---
agent: 'agent'
description: 'Phase 7: Plan the implementation — architecture, connectors, standards, testing'
tools: ['editFiles', 'createFile', 'readFile', 'runInTerminal']
---

You are guiding a developer through Phase 7 of building a Power Apps Code App.

Read the skill file for full guidance: [plan-code skill](../../agents/skills/plan-code/SKILL.md)
Refer to [pac-reference](../../agents/skills/pac-reference.md) for CLI commands.

## Your task

Read `UseCase.md`, `docs/project-brief.md`, and `docs/data-architecture.md`.
Use the DLP policies, environment details, and solution name from UseCase.md
throughout — do not re-ask for information already there.

Create the technical implementation plan:

1. **Architecture decisions** — confirm framework (React default), state management,
   component library, routing based on the capabilities in UseCase.md
2. **Connector manifest** — map every data source from UseCase.md to a specific
   Power Platform connector. For each connector: API name, type (Standard/Premium),
   DLP group (check against UseCase.md DLP info), connection reference name.
   **Flag if Excel Online (Business) or Excel Online (OneDrive) was planned — these
   are not supported in code apps.**
   Generate the `pac code add-data-source` commands using connection references.
3. **Coding standards** — generate these config files in the project root:
   - `tsconfig.json` (strict mode, path aliases, exclude `src/generated/`)
   - `.eslintrc.cjs` (TypeScript strict + jsx-a11y, ignore `src/generated/`)
   - `.prettierrc`
   - `.vscode/settings.json` (format on save, ESLint auto-fix)
   - `.vscode/extensions.json` (recommended extensions)
4. **Error handling strategy** — document the four-layer approach
5. **Test strategy** — document test layers (unit, integration, E2E, accessibility, UAT)
6. **Save** the plan as `docs/implementation-plan.md`, including the full connector
   manifest table and all `pac code add-data-source` commands with connection references

Tell the developer: "Implementation plan complete. Type `/implement-code` to start building."
