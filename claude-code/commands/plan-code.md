# Plan Code

Read `agents/skills/plan-code/SKILL.md` for full guidance, then act directly.

Run this before Phase 6 to lock in technical decisions.

## Steps

### 1. Read context
Read `UseCase.md`, `docs/data-architecture.md`, and `docs/connector-manifest.md`.

Tell the developer:
> "Before we write production code, let's lock in the technical decisions so the
> build phase has a clear plan."

### 2. Framework and state management
Recommend and confirm:
- **Framework**: React + TypeScript (default) or Vue/vanilla TS if requested
- **State management**: Present the comparison table from the SKILL.md
  (Context API vs Zustand vs Redux Toolkit) and recommend based on app complexity
- **Routing**: React Router v6 (default)
- **Component library**: Fluent UI React v9 (default)

### 3. Connector manifest review
Cross-check `docs/connector-manifest.md` against DLP policies in UseCase.md.
Confirm all connectors are in compatible DLP groups. Issue a Go / No-Go.

### 4. Write configuration files
Write the coding standards config files (same as Phase 10) if not already done:
- `tsconfig.json`, `.eslintrc.cjs`, `.prettierrc`, `jest.config.ts`

### 5. Error handling strategy
Confirm the four-layer error architecture from `agents/skills/error-handling/SKILL.md`:
- Global ErrorBoundary
- `withRetry()` connector wrapper
- Hook-level `{ data, isLoading, error, refetch }`
- UI error components

### 6. Test plan
Define coverage targets from `agents/skills/testing/SKILL.md`:
- Unit: 80% line coverage
- Integration: critical paths
- E2E: top 5 user journeys
- Accessibility: zero axe violations

### 7. Write the implementation plan
Write `docs/implementation-plan.md` directly using the template from the SKILL.md.
Include framework decisions, connector manifest summary, error handling strategy,
test plan, and ALM decisions.

When done, say:
> "Implementation Plan saved to `docs/implementation-plan.md`. Run `/implement-code`
> to start building."
