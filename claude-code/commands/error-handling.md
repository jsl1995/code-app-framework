# Phase 11: Error Handling

Read `agents/skills/error-handling/SKILL.md` for full guidance, then act directly.

Implement this during Phase 6 before converting mockups to production components.

## Steps

### 1. Read context
Read `UseCase.md` (for app name and Application Insights key if set) and
`agents/skills/error-handling/SKILL.md` for the full four-layer error architecture.

Tell the developer:
> "[Phase 11/11: Error Handling] I'll implement the four-layer error architecture:
> Global ErrorBoundary → connector retry wrapper → hook-level error states →
> UI error components."

### 2. Write the global ErrorBoundary
Write `src/components/ErrorBoundary.tsx` directly using the Write tool.
Use the class component pattern from the SKILL.md — React error boundaries must
be class components.

### 3. Write the retry utility
Write `src/utils/withRetry.ts` — exponential backoff wrapper for connector calls.
Use the implementation from the SKILL.md.

### 4. Write error UI components
Write `src/components/ErrorMessage.tsx` — reusable error state component with
retry button that accepts `error: Error` and `onRetry: () => void` props.

Write `src/components/PageError.tsx` — full-page error state for route-level errors.

### 5. Apply the hook pattern
Ensure all data-fetching hooks expose `{ data, isLoading, error, refetch }` and
wrap service calls with `withRetry`. Show an example using the first entity from
`docs/data-architecture.md`.

### 6. Set up Application Insights (optional)
If the developer has an Application Insights key:
```bash
npm install @microsoft/applicationinsights-web
```
Write `src/utils/telemetry.ts` using the pattern from the SKILL.md. Log unhandled
errors from the ErrorBoundary and failed connector calls.

### 7. Wrap the app
Update `src/main.tsx` (or `src/App.tsx`) to wrap the app in the global ErrorBoundary.

When done, tell the developer:
> "Error handling layer implemented. All connector errors will be caught,
> retried with backoff, and surfaced to users with a retry button."
