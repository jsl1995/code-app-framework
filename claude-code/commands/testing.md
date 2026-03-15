# Phase 7: Testing

Read `agents/skills/testing/SKILL.md` for full guidance, then act directly.

## Steps

### 1. Read context
Read `UseCase.md`, `docs/project-brief.md`, and the generated service files in
`src/generated/services/`. Extract the entity names and service method signatures.

Tell the developer:
> "[Phase 7/11: Testing] I'll install the test toolchain and generate unit tests,
> integration tests, and Playwright E2E setup for [app name]."

### 2. Install test dependencies
Run using the Bash tool:
```bash
npm install --save-dev \
  jest ts-jest @types/jest jest-environment-jsdom \
  @testing-library/react @testing-library/user-event @testing-library/jest-dom \
  jest-axe @types/jest-axe \
  msw \
  @playwright/test
npx playwright install --with-deps chromium
```

### 3. Write MSW setup files
Write the following files directly:
- `src/test-utils/msw/handlers.ts` — mock handlers for each generated service endpoint
- `src/test-utils/msw/server.ts` — Node server for Jest
- `src/test-utils/setup.ts` — `beforeAll`/`afterEach`/`afterAll` server lifecycle

Use the exact patterns from `agents/skills/testing/SKILL.md`.

### 4. Write Playwright configuration
Write `playwright.config.ts` at the project root with:
- `setup` project for auth state
- `chromium` project depending on setup
- `webServer` pointing to `npm run dev`

Write `e2e/auth.setup.ts` for authentication state capture.

Add to `.gitignore`:
```
playwright/.auth/
playwright-report/
test-results/
```

Update `.env.example` with `TEST_USER_EMAIL`, `TEST_USER_PASSWORD`,
and `PLAYWRIGHT_BASE_URL`.

### 5. Generate unit tests
For each hook in `src/hooks/` and each component in `src/components/`, generate
a test file covering:
1. Loading state
2. Successful render with mock data
3. Error state when service throws
4. Empty state
5. `jest-axe` accessibility assertion

Write tests directly to `src/hooks/[name].test.ts` and
`src/components/[name].test.tsx`.

### 6. Verify coverage
Run:
```bash
npm test -- --coverage --watchAll=false
```

Check that line coverage meets 80% for `src/` (excluding `src/generated/`). Fix
any failing tests before proceeding.

### 7. Write the UAT script
Using the UAT template from the SKILL.md, write `docs/uat-script.md` with test
cases for each core capability from `docs/project-brief.md`.

When done, say:
> "Test suite set up. Coverage meets target. Run `/accessibility` next for the
> formal WCAG 2.2 AA audit."
