# Phase 11: Error Handling & Observability

## Purpose

Define a consistent error handling strategy and observability layer so that
connector failures, runtime errors, and performance issues are caught,
reported, and recoverable. Code apps call external services through connectors —
network and service errors are inevitable, not exceptional.

## Deliverable

An **Error Handling Implementation** and **Observability Configuration** integrated
into the codebase.

## Error Taxonomy for Code Apps

| Category | Source | Example | User Impact |
|----------|--------|---------|-------------|
| Connector Error | Power Platform runtime | Dataverse returns 403, connection expired | Can't load or save data |
| Network Error | Browser / infrastructure | Timeout, offline, DNS failure | App appears broken |
| Validation Error | Client-side logic | Required field empty, invalid format | Form won't submit |
| Auth Error | Entra ID | Token expired, consent revoked | App redirects or fails to load |
| Runtime Error | Application code | Null reference, unhandled promise rejection | White screen or partial render |
| SDK Error | Power Apps client library | SDK init failure, config mismatch | App won't start |

## Error Handling Architecture

### Layer 1: Global Error Boundary

Catches any unhandled React rendering error. Prevents white screen of death.

```typescript
// src/components/ErrorBoundary.tsx
import { Component, ErrorInfo, ReactNode } from 'react';

interface Props {
  children: ReactNode;
  fallback?: ReactNode;
}

interface State {
  hasError: boolean;
  error: Error | null;
}

export class ErrorBoundary extends Component<Props, State> {
  state: State = { hasError: false, error: null };

  static getDerivedStateFromError(error: Error): State {
    return { hasError: true, error };
  }

  componentDidCatch(error: Error, info: ErrorInfo): void {
    // Log to observability layer
    logger.error('Unhandled render error', {
      error: error.message,
      stack: error.stack,
      componentStack: info.componentStack,
    });
  }

  render(): ReactNode {
    if (this.state.hasError) {
      return this.props.fallback ?? (
        <ErrorPage
          title="Something went wrong"
          message="The app encountered an unexpected error."
          onRetry={() => this.setState({ hasError: false, error: null })}
        />
      );
    }
    return this.props.children;
  }
}
```

Wrap the app at the root and optionally around high-risk sections:

```typescript
<ErrorBoundary>
  <App />
</ErrorBoundary>

// More granular:
<ErrorBoundary fallback={<DataLoadError />}>
  <DashboardMetrics />
</ErrorBoundary>
```

### Layer 2: Connector Error Wrapper

Wrap every generated service call to normalise errors and enable retry:

```typescript
// src/services/connectorWrapper.ts

export interface ConnectorError {
  code: string;
  message: string;
  connector: string;
  operation: string;
  isRetryable: boolean;
  raw?: unknown;
}

export async function callConnector<T>(
  connector: string,
  operation: string,
  fn: () => Promise<T>,
  options?: { retries?: number; retryDelay?: number },
): Promise<T> {
  const maxRetries = options?.retries ?? 2;
  const delay = options?.retryDelay ?? 1000;

  for (let attempt = 0; attempt <= maxRetries; attempt++) {
    try {
      return await fn();
    } catch (error: unknown) {
      const connError = normaliseError(error, connector, operation);

      if (!connError.isRetryable || attempt === maxRetries) {
        logger.error('Connector call failed', {
          connector,
          operation,
          attempt,
          error: connError,
        });
        throw connError;
      }

      logger.warn('Connector call failed, retrying', {
        connector,
        operation,
        attempt,
        nextRetryMs: delay * (attempt + 1),
      });

      await sleep(delay * (attempt + 1)); // Exponential backoff
    }
  }

  throw new Error('Unreachable');
}

function normaliseError(
  error: unknown,
  connector: string,
  operation: string,
): ConnectorError {
  const message = error instanceof Error ? error.message : String(error);
  const code = extractErrorCode(error);

  return {
    code,
    message,
    connector,
    operation,
    isRetryable: isRetryableCode(code),
    raw: error,
  };
}

function isRetryableCode(code: string): boolean {
  return ['408', '429', '500', '502', '503', '504', 'NETWORK_ERROR'].includes(code);
}

function sleep(ms: number): Promise<void> {
  return new Promise((resolve) => setTimeout(resolve, ms));
}
```

Usage:
```typescript
const accounts = await callConnector(
  'Dataverse',
  'getAccounts',
  () => DataverseService.getAccounts({ $top: 50 }),
);
```

### Layer 3: Hook-Level Error State

Every data-fetching hook exposes loading, error, and data states:

```typescript
// src/hooks/useConnectorQuery.ts
import { useState, useEffect, useCallback } from 'react';
import { callConnector, ConnectorError } from '../services/connectorWrapper';

interface QueryState<T> {
  data: T | null;
  isLoading: boolean;
  error: ConnectorError | null;
  refetch: () => void;
}

export function useConnectorQuery<T>(
  connector: string,
  operation: string,
  fn: () => Promise<T>,
  deps: unknown[] = [],
): QueryState<T> {
  const [data, setData] = useState<T | null>(null);
  const [isLoading, setIsLoading] = useState(true);
  const [error, setError] = useState<ConnectorError | null>(null);

  const execute = useCallback(async () => {
    setIsLoading(true);
    setError(null);
    try {
      const result = await callConnector(connector, operation, fn);
      setData(result);
    } catch (err) {
      setError(err as ConnectorError);
    } finally {
      setIsLoading(false);
    }
  }, deps);

  useEffect(() => { void execute(); }, [execute]);

  return { data, isLoading, error, refetch: execute };
}
```

### Layer 4: UI Error Components

Standardised error display components:

```typescript
// Inline error for data sections
<ConnectorErrorBanner
  error={error}
  onRetry={refetch}
  context="loading accounts"
/>

// Full page error for critical failures
<ErrorPage
  title="Unable to connect"
  message="We couldn't reach the data source. Check your connection and try again."
  errorCode={error.code}
  onRetry={refetch}
  onGoBack={() => navigate(-1)}
/>

// Toast notification for background operations
<Toast intent="error">
  Failed to save changes. {error.isRetryable ? 'Retrying...' : 'Please try again.'}
</Toast>
```

## Observability

### Logger Abstraction

Use an abstraction so the transport can change without touching app code:

```typescript
// src/services/logger.ts

type LogLevel = 'debug' | 'info' | 'warn' | 'error';

interface LogEntry {
  level: LogLevel;
  message: string;
  context?: Record<string, unknown>;
  timestamp: string;
}

class Logger {
  private transports: Array<(entry: LogEntry) => void> = [];

  addTransport(transport: (entry: LogEntry) => void): void {
    this.transports.push(transport);
  }

  private log(level: LogLevel, message: string, context?: Record<string, unknown>): void {
    const entry: LogEntry = {
      level,
      message,
      context,
      timestamp: new Date().toISOString(),
    };
    this.transports.forEach((t) => t(entry));
  }

  debug(message: string, context?: Record<string, unknown>): void {
    this.log('debug', message, context);
  }
  info(message: string, context?: Record<string, unknown>): void {
    this.log('info', message, context);
  }
  warn(message: string, context?: Record<string, unknown>): void {
    this.log('warn', message, context);
  }
  error(message: string, context?: Record<string, unknown>): void {
    this.log('error', message, context);
  }
}

export const logger = new Logger();

// Development transport — console
if (process.env.NODE_ENV === 'development') {
  logger.addTransport((entry) => {
    const fn = console[entry.level] ?? console.log;
    fn(`[${entry.timestamp}] ${entry.level.toUpperCase()}: ${entry.message}`, entry.context);
  });
}

// Production transport — Application Insights (optional)
// logger.addTransport(appInsightsTransport);
```

### Application Insights Integration (Optional)

For production observability, add Application Insights:

```bash
npm install @microsoft/applicationinsights-web
```

```typescript
// src/services/appInsights.ts
import { ApplicationInsights } from '@microsoft/applicationinsights-web';
import { logger } from './logger';

const appInsights = new ApplicationInsights({
  config: {
    connectionString: process.env.VITE_APPINSIGHTS_CONNECTION_STRING,
    enableAutoRouteTracking: true,
    enableCorsCorrelation: true,
  },
});

appInsights.loadAppInsights();

// Wire into logger
logger.addTransport((entry) => {
  if (entry.level === 'error') {
    appInsights.trackException({
      exception: new Error(entry.message),
      properties: entry.context as Record<string, string>,
    });
  } else if (entry.level === 'warn') {
    appInsights.trackTrace({
      message: entry.message,
      severityLevel: 2,
      properties: entry.context as Record<string, string>,
    });
  }
});
```

### Key Metrics to Track

| Metric | What it tells you | How to capture |
|--------|------------------|----------------|
| App load time | User experience on launch | Performance API / App Insights |
| Connector call latency | Data source responsiveness | Timer in `callConnector` wrapper |
| Connector error rate | Data source reliability | Count errors by connector + operation |
| Active users | Adoption and usage | App Insights page views |
| Unhandled exceptions | Code quality | Error boundary + global handlers |
| Client-side errors | Browser compatibility | `window.onerror` + `unhandledrejection` |

### Global Unhandled Error Handlers

Catch anything that escapes React's error boundaries:

```typescript
// src/main.tsx (app entry point)
window.addEventListener('unhandledrejection', (event) => {
  logger.error('Unhandled promise rejection', {
    reason: String(event.reason),
    stack: event.reason?.stack,
  });
});

window.addEventListener('error', (event) => {
  logger.error('Unhandled error', {
    message: event.message,
    filename: event.filename,
    lineno: event.lineno,
  });
});
```

## GitHub Copilot Prompt

```
@workspace Create a ConnectorErrorBanner component that:
- Accepts a ConnectorError and a retry callback
- Shows a Fluent UI MessageBar with the error message
- Maps error codes to user-friendly messages (403 = permissions,
  404 = not found, 429 = rate limited, 500+ = service issue)
- Shows a "Retry" button for retryable errors
- Announces the error to screen readers via aria-live
- Includes a "Details" expander for technical info (connector, operation, code)
Use TypeScript and follow our project coding standards.
```
