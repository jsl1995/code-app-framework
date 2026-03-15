# Changelog

All notable changes to the **code-app-framework** are recorded here.

Framework changes (skill updates, new phases, structural changes) are versioned here.
Application code generated using this framework is not versioned here.

---

## [1.2.0] — 2026-03-15

### Added

- `claude-code/CLAUDE.md` — Claude Code root orchestrator (equivalent of `AGENTS.md`
  for GitHub Copilot). Copy to project root to use with Claude Code.
- `claude-code/commands/` — 13 Claude Code slash commands covering all 11 phases
  plus supplemental skills. Copy to `.claude/commands/` to enable.
  Commands are **action-oriented**: Claude Code reads, writes files, and runs PAC CLI
  and npm commands directly — unlike Copilot prompts which generate text for the
  developer to run.
- `README.md` — "Which AI tool are you using?" section with side-by-side comparison
  table and full Claude Code setup and slash command reference

### Changed

- `README.md` — project structure tree updated to show `claude-code/` folder alongside
  `.github/` with clear annotations for both entry points
- `README.md` — Getting started Step 2 now references `UseCase.example.md`

---

## [1.1.0] — 2026-03-15

### Added

- `UseCase.example.md` — fully filled example use case (Asset Tracker app) to show
  the expected level of detail in `UseCase.md`
- `.github/prompts/architecture.prompt.md` — VS Code `/architecture` slash command
  for Phase 2 (was previously only accessible via Agent Mode)
- `.github/prompts/connectors.prompt.md` — VS Code `/connectors` slash command
  for Phase 5 (was previously only accessible via Agent Mode)
- `agents/skills/connectors/SKILL.md` — state management patterns section: hook
  pattern, avoiding redundant connector calls, cache invalidation after mutations
- `agents/skills/testing/SKILL.md` — full toolchain setup: install commands,
  MSW handler + server setup, integration test pattern, `playwright.config.ts`
  template, and Playwright auth state setup
- `agents/skills/implement-code/SKILL.md` — expanded CI/CD section: service
  principal creation steps, Power Platform permission assignment, GitHub secrets
  setup, and a complete GitHub Actions workflow with test gate before deploy
- `agents/skills/implement-code/SKILL.md` — `power.config.json` explanation and
  env variable documentation (`.env.local` pattern, `.env.example` guidance)
- `AGENTS.md` — Entry points section documenting both Agent Mode and VS Code
  custom agent entry points with slash command table
- `AGENTS.md` — Phase numbering cross-reference table mapping the 11-phase
  Agent Mode model to the 8-phase VS Code `code-app-builder` model
- `README.md` — VS Code custom agent and slash commands documentation

### Fixed

- `agents/skills/coding-standards/SKILL.md` — corrected Jest config key from
  `setupFilesAfterFramework` (invalid) to `setupFilesAfterEnv` (correct Jest option)

### Changed

- `AGENTS.md` — updated skill status table: phases 2, 5, 7, 8, 9, 10, and 11
  were incorrectly marked as "Stub" — all are now marked "Ready"

---

## [1.0.0] — 2026-01-01

### Initial release

Full 11-phase framework covering:

- **Phase 1** Brainstorming — UseCase.md validation and Project Brief
- **Phase 2** Architecture — component diagram, ALM, security posture
- **Phase 3** Data Structure — ER diagrams, table schemas, delegation
- **Phase 4** Mock-ups — working React/TS components with Fluent UI
- **Phase 5** Connectors — connector manifest and connection references
- **Phase 6** Implement Code — scaffold, Copilot prompt sequences, build, deploy
- **Phase 7** Testing — unit, integration, E2E, accessibility, UAT strategy
- **Phase 8** Accessibility — WCAG 2.2 AA audit with Fluent UI-specific checks
- **Phase 9** Governance — handover pack, ops runbook, naming conventions
- **Phase 10** Coding Standards — ESLint, Prettier, TypeScript strict, Husky
- **Phase 11** Error Handling — ErrorBoundary, retry logic, Application Insights

Supporting skills: `create-dataverse`, `plan-code`, `pac-reference`,
`power-apps-code-apps` (master platform reference)

**Validated against:** PAC CLI 1.34, `@microsoft/power-apps` npm package v1.0.4+,
Power Apps Code Apps initial release (2025), Fluent UI React v9

---

> To check the PAC CLI version: `pac --version`
> To check the npm package version: `npm list @microsoft/power-apps`
