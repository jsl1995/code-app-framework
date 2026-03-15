# Phase 7: Testing & QA

## Purpose

Validate that the app works correctly, performs acceptably, and meets the success
criteria defined in Phase 1. Testing runs throughout the build phase — this skill
documents the strategy, tooling, and coverage targets.

## Deliverable

A **Test Report** confirming coverage targets met, all critical paths pass, and
UAT sign-off obtained.

## Test Layers

| Layer | Tool | What to test | Coverage target |
|-------|------|-------------|-----------------|
| Unit | Jest + React Testing Library | Components, hooks, utils | 80% lines |
| Integration | RTL + MSW | Navigation flows, form submissions | Critical paths |
| E2E | Playwright | Auth flow, top 5 user journeys | Happy paths |
| Accessibility | axe-core + jest-axe | Every component | Zero violations |
| UAT | Manual | All capabilities from project brief | Sign-off |

## Key Testing Patterns

### Unit tests

- Mock generated services with `jest.mock()` — never call real connectors in unit tests
- Test components in isolation: loading, error, empty, and populated states
- Exclude `generated/` from coverage reporting (`jest.config.ts` `coveragePathIgnorePatterns`)

```typescript
// Example: mock a generated service
jest.mock('./generated/services/AccountsService');
import { AccountsService } from './generated/services/AccountsService';

(AccountsService.getall as jest.Mock).mockResolvedValue({ data: mockAccounts });
```

### Integration tests

- Use MSW (Mock Service Worker) to intercept connector HTTP calls
- Test navigation between pages
- Test form submission end-to-end (fill → validate → submit → success/error state)

### E2E tests (Playwright)

- Pre-authenticate with stored auth state to avoid repeated login
- Cover the top 5 user journeys from the project brief
- Run against the Dev environment after each deployment

```typescript
// Example: store auth state
await page.context().storageState({ path: 'playwright/.auth/user.json' });
```

### Accessibility tests

- Run `jest-axe` on every component render
- Zero violations is a hard gate — do not ship with known axe violations
- Run Playwright + axe on full page renders for integration-level checks

```typescript
import { axe, toHaveNoViolations } from 'jest-axe';
expect.extend(toHaveNoViolations);

it('has no accessibility violations', async () => {
  const { container } = render(<AccountList accounts={mockAccounts} />);
  expect(await axe(container)).toHaveNoViolations();
});
```

## Toolchain Setup

### Install dependencies

```bash
# Jest + React Testing Library
npm install --save-dev jest ts-jest @types/jest jest-environment-jsdom \
  @testing-library/react @testing-library/user-event @testing-library/jest-dom \
  jest-axe @types/jest-axe

# MSW (Mock Service Worker) for integration tests
npm install --save-dev msw

# Playwright for E2E
npm install --save-dev @playwright/test
npx playwright install --with-deps chromium
```

### MSW setup

Create a handler file and a browser/node entry point:

```typescript
// src/test-utils/msw/handlers.ts
import { http, HttpResponse } from 'msw';

export const handlers = [
  // Mock connector HTTP calls — match the Power Platform connector proxy URL pattern
  http.get('*/api/data/v9.2/accounts', () => {
    return HttpResponse.json({
      value: [
        { accountid: '1', name: 'Contoso Ltd', statuscode: 1 },
        { accountid: '2', name: 'Fabrikam Inc', statuscode: 1 },
      ],
    });
  }),
];
```

```typescript
// src/test-utils/msw/server.ts  (Node — used by Jest)
import { setupServer } from 'msw/node';
import { handlers } from './handlers';

export const server = setupServer(...handlers);
```

```typescript
// src/test-utils/setup.ts  (referenced by jest.config.ts setupFilesAfterEnv)
import '@testing-library/jest-dom';
import { server } from './msw/server';

beforeAll(() => server.listen({ onUnhandledRequest: 'warn' }));
afterEach(() => server.resetHandlers());
afterAll(() => server.close());
```

### Integration test pattern (RTL + MSW)

```typescript
import { render, screen, waitFor } from '@testing-library/react';
import userEvent from '@testing-library/user-event';
import { MemoryRouter } from 'react-router-dom';
import { AccountList } from '../components/AccountList';

it('loads and displays accounts', async () => {
  render(
    <MemoryRouter>
      <AccountList />
    </MemoryRouter>
  );

  expect(screen.getByRole('progressbar')).toBeInTheDocument(); // loading state

  await waitFor(() => {
    expect(screen.getByText('Contoso Ltd')).toBeInTheDocument();
    expect(screen.getByText('Fabrikam Inc')).toBeInTheDocument();
  });
});
```

### Playwright configuration

```typescript
// playwright.config.ts
import { defineConfig, devices } from '@playwright/test';

export default defineConfig({
  testDir: './e2e',
  fullyParallel: true,
  forbidOnly: !!process.env.CI,
  retries: process.env.CI ? 2 : 0,
  reporter: 'html',
  use: {
    baseURL: process.env.PLAYWRIGHT_BASE_URL ?? 'http://localhost:5173',
    trace: 'on-first-retry',
  },
  projects: [
    // Auth setup — runs once to store authenticated state
    {
      name: 'setup',
      testMatch: /.*\.setup\.ts/,
    },
    {
      name: 'chromium',
      use: {
        ...devices['Desktop Chrome'],
        storageState: 'playwright/.auth/user.json',
      },
      dependencies: ['setup'],
    },
  ],
  webServer: {
    command: 'npm run dev',
    url: 'http://localhost:5173',
    reuseExistingServer: !process.env.CI,
  },
});
```

### Playwright auth setup

```typescript
// e2e/auth.setup.ts
import { test as setup, expect } from '@playwright/test';
import path from 'path';

const authFile = path.join(__dirname, '../playwright/.auth/user.json');

setup('authenticate', async ({ page }) => {
  // Navigate to the app — the Power Apps host redirects to Entra ID login
  await page.goto('/');

  // Fill in credentials (store in environment variables, never hardcode)
  await page.getByLabel('Email').fill(process.env.TEST_USER_EMAIL!);
  await page.getByLabel('Password').fill(process.env.TEST_USER_PASSWORD!);
  await page.getByRole('button', { name: 'Sign in' }).click();

  // Wait for the app to load after auth
  await expect(page.getByRole('main')).toBeVisible({ timeout: 30_000 });

  // Save auth state so all other tests reuse it
  await page.context().storageState({ path: authFile });
});
```

Add to `.gitignore`:
```
playwright/.auth/
playwright-report/
test-results/
```

Add to `.env.example`:
```bash
TEST_USER_EMAIL=        # Service account email for Playwright E2E tests
TEST_USER_PASSWORD=     # Service account password for Playwright E2E tests
PLAYWRIGHT_BASE_URL=    # App URL for E2E tests (default: http://localhost:5173)
```

## Performance Targets

| Metric | Target |
|--------|--------|
| Initial load (typical connection) | < 3 seconds |
| Connector call response | < 2 seconds for list queries |
| Time to interactive | < 4 seconds |

## UAT Script Template

```markdown
## UAT Script: [Capability Name]

**Tester**:
**Date**:
**Environment**:

### Steps
1. Navigate to [URL]
2. [Action]
3. [Expected result]

### Pass / Fail:
### Notes:
```

## GitHub Copilot Prompt

```
@workspace Generate unit tests for the [ComponentName] component.
Use Jest and React Testing Library. Mock [ServiceName] using jest.mock().
Test the following cases:
1. Loading state while data is being fetched
2. Successful render with mock data
3. Error state when the service throws
4. Empty state when the service returns an empty array
Reference #file:generated/services/[ServiceName].ts for the mock interface.
```

## Transition to Phase 8

Once test coverage targets are met and UAT is signed off, proceed to
`agents/skills/accessibility/SKILL.md` for the formal accessibility audit.
