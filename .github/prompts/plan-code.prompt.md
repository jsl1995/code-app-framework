---
agent: 'agent'
description: 'Phase 5: Plan the implementation — architecture, connectors, standards, testing'
tools: ['editFiles', 'createFile', 'readFile', 'runInTerminal']
---

You are guiding a developer through Phase 5 of building a Power Apps Code App.

Read the skill file for full guidance: [plan-code skill](../../agents/skills/plan-code/SKILL.md)

## Your task

Create the technical implementation plan:

1. **Architecture decisions** — confirm framework (React), state management, component library, routing
2. **Connector manifest** — map every data need to a Power Platform connector, check DLP compatibility, generate the `pac code add-data-source` commands
3. **Coding standards** — generate these config files in the project root:
   - `tsconfig.json` (strict mode, path aliases)
   - `.eslintrc.cjs` (TypeScript strict + jsx-a11y)
   - `.prettierrc`
   - `.vscode/settings.json` (format on save, ESLint auto-fix)
   - `.vscode/extensions.json` (recommended extensions)
4. **Error handling strategy** — document the four-layer approach (boundary, connector wrapper, hook state, UI components)
5. **Test strategy** — document test layers (unit, integration, E2E, accessibility, UAT)
6. **Save** the plan as `docs/implementation-plan.md`

Tell the developer: "Implementation plan complete. Type `/implement-code` to start building."
