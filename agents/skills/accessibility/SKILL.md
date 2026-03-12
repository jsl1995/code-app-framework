# Phase 8: Accessibility & Compliance

## Purpose

Ensure the code app meets WCAG 2.2 AA standards (minimum), organisational
accessibility policies, and any sector-specific compliance requirements.
Because code apps are full SPAs with custom UI, accessibility is entirely
your responsibility — the platform provides auth and hosting, but not
accessible components.

## Deliverable

An **Accessibility Compliance Report** and a verified, remediated codebase.

## Why This Matters for Code Apps Specifically

With canvas and model-driven apps, Microsoft's built-in controls carry some
baseline accessibility. Code apps give you total UI freedom, which means total
accessibility responsibility. If you're delivering into UK public sector
(which many Avanade engagements touch), the Public Sector Bodies Accessibility
Regulations 2018 legally require WCAG 2.1 AA compliance for internal tools.

## WCAG 2.2 AA Checklist for Code Apps

### Perceivable

| Criterion | What to check | How to verify |
|-----------|--------------|---------------|
| 1.1.1 Non-text content | All images, icons, charts have alt text | Audit all `<img>`, `<svg>`, icon components |
| 1.3.1 Info and relationships | Semantic HTML: headings, lists, tables, landmarks | Inspect DOM structure, check for `<header>`, `<nav>`, `<main>`, `<h1>`–`<h6>` hierarchy |
| 1.3.2 Meaningful sequence | Reading order matches visual order | Tab through the page with keyboard, verify logical order |
| 1.4.1 Use of colour | Colour is not the only indicator (e.g., error states) | Review all status indicators — do they use icons/text alongside colour? |
| 1.4.3 Contrast minimum | 4.5:1 for normal text, 3:1 for large text | Run axe DevTools or Lighthouse |
| 1.4.4 Resize text | Content usable at 200% zoom | Zoom browser to 200%, check for overflow/truncation |
| 1.4.10 Reflow | No horizontal scrolling at 320px width | Test at 320px viewport width |

### Operable

| Criterion | What to check | How to verify |
|-----------|--------------|---------------|
| 2.1.1 Keyboard | All functionality accessible via keyboard | Tab through entire app, activate every control with Enter/Space |
| 2.1.2 No keyboard trap | Focus can always move away from any element | Tab through modals, dropdowns, date pickers |
| 2.4.1 Bypass blocks | Skip navigation link or landmark regions | Check for skip link or proper `<nav>`, `<main>` landmarks |
| 2.4.3 Focus order | Focus order is logical and predictable | Tab through pages, verify focus follows visual layout |
| 2.4.6 Headings and labels | Headings describe content, labels describe inputs | Review all headings and form labels |
| 2.4.7 Focus visible | Keyboard focus indicator is always visible | Tab through app in default browser styles — is focus ring visible? |
| 2.4.11 Focus not obscured | Focused element is not hidden behind sticky headers/menus | Tab with sticky header present |

### Understandable

| Criterion | What to check | How to verify |
|-----------|--------------|---------------|
| 3.1.1 Language of page | `lang` attribute on `<html>` | Inspect document root |
| 3.2.1 On focus | No unexpected context change on focus | Tab through all elements |
| 3.3.1 Error identification | Form errors identified and described in text | Submit invalid forms, check error messages |
| 3.3.2 Labels or instructions | All form inputs have visible labels | Review every form field |

### Robust

| Criterion | What to check | How to verify |
|-----------|--------------|---------------|
| 4.1.2 Name, role, value | Custom components expose correct ARIA attributes | Run axe DevTools, inspect custom widgets |
| 4.1.3 Status messages | Dynamic content changes announced to screen readers | Use NVDA/VoiceOver to test toast notifications, loading states |

## Implementation Standards

### Component-Level Requirements

Every interactive component must have:

```typescript
// Accessible button example
<button
  aria-label="Delete item: Contoso Ltd"  // Descriptive label
  aria-describedby="delete-warning"       // Additional context
  onClick={handleDelete}
>
  <TrashIcon aria-hidden="true" />        // Decorative icons hidden
  Delete
</button>

// Accessible form field example
<div role="group" aria-labelledby="contact-heading">
  <h2 id="contact-heading">Contact Details</h2>
  <label htmlFor="email">Email address</label>
  <input
    id="email"
    type="email"
    aria-required="true"
    aria-invalid={!!errors.email}
    aria-describedby={errors.email ? 'email-error' : undefined}
  />
  {errors.email && (
    <span id="email-error" role="alert">{errors.email}</span>
  )}
</div>
```

### ARIA Live Regions for Dynamic Content

Code apps fetch data asynchronously. Screen reader users need to know when
content changes:

```typescript
// Announce data loading/loaded states
<div aria-live="polite" aria-atomic="true">
  {isLoading && <span>Loading accounts...</span>}
  {data && <span>{data.length} accounts loaded</span>}
  {error && <span role="alert">Error loading accounts: {error.message}</span>}
</div>
```

### Focus Management on Navigation

When the SPA route changes, focus must move to the new content:

```typescript
const mainRef = useRef<HTMLElement>(null);

useEffect(() => {
  // Move focus to main content on route change
  mainRef.current?.focus();
}, [location.pathname]);

<main ref={mainRef} tabIndex={-1} aria-label="Page content">
  {/* Route outlet */}
</main>
```

### Skip Navigation Link

```typescript
// First element in the app, before the header
<a href="#main-content" className="skip-link">
  Skip to main content
</a>

// CSS
.skip-link {
  position: absolute;
  left: -9999px;
  top: auto;
  width: 1px;
  height: 1px;
  overflow: hidden;
}
.skip-link:focus {
  position: fixed;
  top: 0; left: 0;
  width: auto; height: auto;
  z-index: 9999;
  padding: 8px 16px;
  background: #fff;
  color: #000;
}
```

## Automated Testing for Accessibility

### axe-core integration with Jest

```bash
npm install --save-dev jest-axe @axe-core/react
```

```typescript
import { axe, toHaveNoViolations } from 'jest-axe';
import { render } from '@testing-library/react';

expect.extend(toHaveNoViolations);

test('AccountList has no accessibility violations', async () => {
  const { container } = render(<AccountList accounts={mockAccounts} />);
  const results = await axe(container);
  expect(results).toHaveNoViolations();
});
```

### Playwright accessibility testing

```typescript
import AxeBuilder from '@axe-core/playwright';

test('dashboard page passes accessibility audit', async ({ page }) => {
  await page.goto('/');
  const results = await new AxeBuilder({ page }).analyze();
  expect(results.violations).toEqual([]);
});
```

### CI Pipeline Integration

Add to your build pipeline so accessibility regressions block deployment:

```yaml
- name: Run accessibility tests
  run: npm run test -- --testPathPattern="a11y"
```

## Manual Testing Checklist

Automated tools catch about 30–40% of accessibility issues. Manual testing is essential:

- [ ] Keyboard-only navigation: complete every user journey without a mouse
- [ ] Screen reader testing: NVDA (Windows) or VoiceOver (macOS) through critical paths
- [ ] High contrast mode: Windows high contrast, ensure all content visible
- [ ] 200% zoom: no content loss or overlap
- [ ] 320px viewport: content reflows without horizontal scroll
- [ ] Reduced motion: `prefers-reduced-motion` media query respected (no essential animations)
- [ ] Touch targets: all interactive elements at least 44×44px on mobile

## Compliance Report Template

```markdown
# Accessibility Compliance Report: [App Name]

**Date**: 
**Assessor**: 
**Standard**: WCAG 2.2 Level AA
**Tools used**: axe DevTools, Lighthouse, NVDA, keyboard testing

## Summary
- Total criteria assessed: 
- Criteria met: 
- Criteria partially met: 
- Criteria not met: 

## Findings

| # | Criterion | Status | Finding | Remediation | Priority |
|---|-----------|--------|---------|-------------|----------|
| 1 | 1.4.3 Contrast | Fail | Submit button contrast ratio 3.2:1 | Darken button background to #0054a6 | High |

## Remediation Plan
| # | Fix | Owner | Target Date | Status |
|---|-----|-------|-------------|--------|
| 1 | Update button colour | Dev | [date] | Open |

## Sign-off
Accessibility reviewed and approved by: _________________ Date: ___________
```

## GitHub Copilot Prompt

```
@workspace Audit #file:src/components/CreateForm.tsx for WCAG 2.2 AA compliance.
Check for: proper label associations, aria attributes on inputs, error message
announcements, keyboard operability, focus management, and colour contrast.
List any issues found and provide the corrected code.
```
