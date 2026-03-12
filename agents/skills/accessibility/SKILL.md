# Phase 8: Accessibility & Compliance

## Purpose

Ensure the app meets WCAG 2.2 AA accessibility standards before release. This is a
release gate — not an afterthought. Accessibility issues found here must be fixed
before proceeding to governance and handover.

## Deliverable

An **Accessibility Audit Report** documenting each tested component, any violations
found, fixes applied, and final pass confirmation.

## WCAG 2.2 AA Key Requirements

| Principle | Key requirements |
|-----------|----------------|
| Perceivable | Text alternatives for images, captions for media, sufficient colour contrast (4.5:1 normal text, 3:1 large text) |
| Operable | All functionality keyboard-accessible, no keyboard traps, skip navigation links |
| Understandable | Labels on all form inputs, clear error messages, consistent navigation |
| Robust | Valid HTML, ARIA roles used correctly, works with screen readers |

## Audit Checklist

### Keyboard navigation
- [ ] All interactive elements reachable by Tab key
- [ ] Focus order is logical (matches visual layout)
- [ ] No keyboard traps (Tab can always exit a component)
- [ ] Skip-to-content link present
- [ ] Modal dialogs trap focus correctly and return focus on close

### Screen reader compatibility
- [ ] All images have meaningful `alt` text (or `alt=""` for decorative)
- [ ] Form inputs have associated `<label>` or `aria-label`
- [ ] Error messages are announced (use `aria-live` or `role="alert"`)
- [ ] Tables have `<th>` elements with `scope` attributes
- [ ] Icons used as buttons have `aria-label`

### Colour and contrast
- [ ] Normal text: contrast ratio ≥ 4.5:1
- [ ] Large text (18pt / 14pt bold): contrast ratio ≥ 3:1
- [ ] UI components (borders, focus indicators): contrast ratio ≥ 3:1
- [ ] Information is not conveyed by colour alone

### Fluent UI specific
- [ ] Use Fluent UI's built-in accessibility props (`ariaLabel`, `ariaDescription`)
- [ ] `DetailsList` — verify column headers are announced correctly
- [ ] `Dialog` — verify focus trap and close behaviour
- [ ] `CommandBar` — verify keyboard navigation and ARIA roles

## Automated Testing

Run axe on every component and every full page render:

```bash
# Install
npm install --save-dev jest-axe @axe-core/playwright

# Run unit-level accessibility tests
npm test -- --testPathPattern="*.test.tsx"

# Run Playwright accessibility scan
npx playwright test --grep "@a11y"
```

```typescript
// Playwright example: full page axe scan
import { checkA11y } from 'axe-playwright';

test('home page has no accessibility violations', async ({ page }) => {
  await page.goto('/');
  await checkA11y(page);
});
```

## Audit Report Template

```markdown
# Accessibility Audit: [App Name]
**Date**:
**Auditor**:
**Standard**: WCAG 2.2 AA
**Tools**: jest-axe, axe-playwright, manual keyboard testing, NVDA/VoiceOver

## Summary
- Components tested:
- Violations found:
- Violations resolved:
- Outstanding issues:

## Findings

### [Component Name]
| Issue | WCAG Criterion | Severity | Status |
|-------|---------------|----------|--------|
| | | Critical / Serious / Moderate | Fixed / Accepted |

## Sign-off
- [ ] All Critical and Serious violations resolved
- [ ] Moderate violations reviewed and dispositioned
- [ ] Screen reader testing passed (NVDA + Chrome, VoiceOver + Safari)
- [ ] Keyboard-only navigation tested end-to-end
```

## GitHub Copilot Prompt

```
@workspace Audit the [ComponentName] component for WCAG 2.2 AA compliance.
Check for:
1. Missing aria-label on icon buttons
2. Form inputs without associated labels
3. Colour contrast issues in inline styles
4. Missing alt text on images
5. Incorrect ARIA roles
Suggest specific code fixes for each issue found.
```

## Transition to Phase 9

Once the audit report shows zero Critical/Serious violations and sign-off is obtained,
proceed to `agents/skills/governance/SKILL.md` for documentation and handover.
