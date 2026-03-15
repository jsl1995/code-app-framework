# Phase 8: Accessibility

Read `agents/skills/accessibility/SKILL.md` for full guidance, then act directly.

## Steps

### 1. Read context
Read `UseCase.md` (for compliance requirements) and scan all files in
`src/components/` using Glob. Tell the developer:
> "[Phase 8/11: Accessibility] I'll audit every component for WCAG 2.2 AA compliance
> and produce a signed-off accessibility report."

### 2. Run axe-core via jest-axe
Confirm jest-axe assertions are present in all component test files. If any
component test is missing an axe assertion, add it now:
```typescript
import { axe, toHaveNoViolations } from 'jest-axe';
expect.extend(toHaveNoViolations);

it('has no accessibility violations', async () => {
  const { container } = render(<ComponentName {...mockProps} />);
  expect(await axe(container)).toHaveNoViolations();
});
```

Run `npm test -- --watchAll=false` and check for axe violations. Fix all violations
before continuing — zero violations is a hard gate.

### 3. Manual checklist review
Go through the checklist in the SKILL.md against the actual component files:

**Keyboard navigation** — check interactive elements have visible focus styles,
tab order is logical, modals trap focus, no keyboard traps elsewhere.

**Screen reader** — verify meaningful `aria-label` on icon buttons, Fluent UI
`DetailsList` has `aria-label`, dialogs have `aria-labelledby`.

**Colour contrast** — Fluent UI v9 tokens are WCAG AA compliant by default; flag
any custom colour overrides for manual verification.

**Fluent UI specifics** — verify `CommandBar` items have `ariaLabel`, `DatePicker`
has associated `<label>`, `Spinner` has `label` prop.

### 4. Fix violations
For each issue found, edit the relevant component file directly using the Edit tool.
Document what was changed and why in the audit report.

### 5. Write the accessibility audit report
Write `docs/accessibility-audit.md` directly using the template from the SKILL.md:
- Summary (pass / fail per section)
- Zero axe violations confirmed
- Manual checklist results
- Any open items with severity triage
- Sign-off statement

When done, say:
> "Accessibility audit complete and saved to `docs/accessibility-audit.md`. Zero
> axe violations confirmed. Run `/governance` next for handover documentation."
