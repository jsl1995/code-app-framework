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
