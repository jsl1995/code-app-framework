# Phase 11: Error Handling & Observability

## Purpose

Define and implement a consistent, layered error handling strategy so that connector
failures, network issues, and unexpected runtime errors never leave users with a blank
screen or a cryptic message. Set this up at the start of Phase 6 alongside coding standards.

## Deliverable

Error handling implemented across all four layers (boundary, connector wrapper, hook,
UI) with a logging strategy configured for both development and production.

## Error Layers

| Layer | Scope | Implementation |
|-------|-------|---------------|
| 1. Global Error Boundary | Unhandled React rendering errors | `ErrorBoundary` component wrapping the app root |
| 2. Connector Wrapper | Every generated service call | `withRetry()` utility with normalised error types |
| 3. Hook-Level State | Per-feature data fetching | `{ data, isLoading, error, refetch }` pattern |
| 4. UI Error Components | User-facing messages | `ErrorBanner`, `ErrorPage`, `Toast` components |

## Error Taxonomy

Classify errors before deciding how to handle them:

| Class | HTTP codes | Retryable? | User message |
|-------|-----------|-----------|--------------|
| Transient | 408, 429, 500, 502, 503, 504 | Yes | "Something went wrong. Retrying…" |
| Auth | 401 | No — redirect to re-auth | "Your session has expired. Please sign in again." |
| Forbidden | 403 | No | "You don't have permission to do that." |
| Not found | 404 | No | "This item no longer exists." |
| Validation | 400 | No — show field errors | [field-specific messages] |
| Unknown | Other | No | "An unexpected error occurred. Please try again." |

## Retry Policy

```typescript
// src/utils/withRetry.ts
const RETRYABLE_CODES = [408, 429, 500, 502, 503, 504];
const MAX_RETRIES = 2;

export async function withRetry<T>(
  fn: () => Promise<T>,
  retries = MAX_RETRIES,
): Promise<T> {
  try {
    return await fn();
  } catch (error) {
    const status = (error as { status?: number }).status;
    if (retries > 0 && status && RETRYABLE_CODES.includes(status)) {
      const delay = (MAX_RETRIES - retries + 1) * 1000; // 1s, 2s
      await new Promise(r => setTimeout(r, delay));
      return withRetry(fn, retries - 1);
    }
    throw error;
  }
}
```

## Global Error Boundary

```tsx
// src/components/ErrorBoundary.tsx
import { Component, type ReactNode } from 'react';

interface Props { children: ReactNode; fallback?: ReactNode; }
interface State { hasError: boolean; error?: Error; }

export class ErrorBoundary extends Component<Props, State> {
  state: State = { hasError: false };

  static getDerivedStateFromError(error: Error): State {
    return { hasError: true, error };
  }

  componentDidCatch(error: Error, info: { componentStack: string }) {
    logger.error('Unhandled render error', { error, componentStack: info.componentStack });
  }

  render() {
    if (this.state.hasError) {
      return this.props.fallback ?? <ErrorPage error={this.state.error} />;
    }
    return this.props.children;
  }
}
```

## Hook Pattern

Every data-fetching hook exposes a consistent interface:

```typescript
// Example: src/hooks/useAccounts.ts
import { useState, useEffect, useCallback } from 'react';
import { AccountsService } from '@/generated/services/AccountsService';
import { withRetry } from '@utils/withRetry';
import type { Account } from '@/generated/models/AccountsModel';

interface UseAccountsResult {
  data: Account[];
  isLoading: boolean;
  error: Error | null;
  refetch: () => void;
}

export function useAccounts(): UseAccountsResult {
  const [data, setData] = useState<Account[]>([]);
  const [isLoading, setIsLoading] = useState(true);
  const [error, setError] = useState<Error | null>(null);

  const fetch = useCallback(async () => {
    setIsLoading(true);
    setError(null);
    try {
      const result = await withRetry(() => AccountsService.getall());
      setData(result.data ?? []);
    } catch (err) {
      setError(err instanceof Error ? err : new Error(String(err)));
    } finally {
      setIsLoading(false);
    }
  }, []);

  useEffect(() => { void fetch(); }, [fetch]);

  return { data, isLoading, error, refetch: fetch };
}
```

## Logging Strategy

```typescript
// src/utils/logger.ts
type LogLevel = 'debug' | 'info' | 'warn' | 'error';

interface LogEntry {
  level: LogLevel;
  message: string;
  data?: unknown;
  timestamp: string;
}

const logger = {
  debug: (message: string, data?: unknown) => log('debug', message, data),
  info:  (message: string, data?: unknown) => log('info', message, data),
  warn:  (message: string, data?: unknown) => log('warn', message, data),
  error: (message: string, data?: unknown) => log('error', message, data),
};

function log(level: LogLevel, message: string, data?: unknown) {
  const entry: LogEntry = { level, message, data, timestamp: new Date().toISOString() };

  // Console transport (always)
  console[level](entry);

  // Application Insights transport (production)
  if (typeof window !== 'undefined' && window.appInsights) {
    if (level === 'error') window.appInsights.trackException({ exception: data as Error });
    else window.appInsights.trackTrace({ message, severityLevel: levelToSeverity(level) });
  }
}

export default logger;
```

## Application Insights (Optional — Production)

```bash
npm install @microsoft/applicationinsights-web
```

```typescript
// src/main.tsx — initialise before rendering
import { ApplicationInsights } from '@microsoft/applicationinsights-web';

if (import.meta.env.PROD) {
  const appInsights = new ApplicationInsights({
    config: { connectionString: import.meta.env.VITE_APPINSIGHTS_CONNECTION_STRING },
  });
  appInsights.loadAppInsights();
  appInsights.trackPageView();
  window.appInsights = appInsights;
}
```

Add `VITE_APPINSIGHTS_CONNECTION_STRING` to `.env.production` (not committed to source control).

## Troubleshooting Table (for implement-code)

| Problem | Likely Cause | Fix |
|---------|-------------|-----|
| Connector call returns 401 | Token expired | Re-authenticate: `pac auth create` |
| Connector call returns 403 | Missing permission | Check Dataverse security role or SQL user permissions |
| All connector calls fail locally | PAC auth not set | Run `pac auth create --environment <url>` |
| App loads but data is empty | Generated service missing | Run `pac code add-data-source` for the connector |
| Schema mismatch error | Connector schema changed | Delete data source and re-add it |
| DLP error on app start | Connector grouping violation | Adjust DLP policy or move connector to correct group |

## GitHub Copilot Prompt

```
@workspace Implement the error handling layer for this code app:
1. A withRetry utility in src/utils/withRetry.ts — retries on 408/429/500/502/503/504,
   max 2 retries, exponential backoff (1s, 2s)
2. A logger utility in src/utils/logger.ts — console transport always,
   Application Insights transport in production
3. A global ErrorBoundary component in src/components/ErrorBoundary.tsx —
   catches render errors, logs them, shows a friendly ErrorPage fallback
4. An ErrorBanner component for inline connector errors (dismissible)
Use TypeScript strict mode throughout. Reference the retry policy in the technical plan.
```
