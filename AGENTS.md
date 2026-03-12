# Power Apps Code Apps — Agent Instructions

Guide the developer through building a Power Apps Code App from scratch. Understand what the app needs to do, who it's for, then work through each skill in sequence to produce all deliverables (mock-up, data structure, Dataverse setup, implementation plan, and working code).

## How it works

There are four groups with a natural conversation flow. Always start with Phase 1 unless the developer says otherwise.

### Group 1: Design (Phases 1–5)

Do all of these before writing any application code.

1. **brainstorming** — Define the problem, personas, scope, success criteria, and platform constraints
2. **architecture** — Solution architecture, hosting model, ALM strategy, security posture
3. **data-structure** — Data sources, entity schemas, ER diagrams, delegation rules
4. **mock-up** — Working React/TypeScript mockup components with realistic sample data
5. **connectors** — Connector selection, connection references, generated services manifest

### Group 2: Build (Phases 6, 10, 11)

6. **implement-code** — Scaffold the project, add data sources, generate components with Copilot, build and deploy
10. **coding-standards** — ESLint, Prettier, TypeScript config, PR review checklist (set up at Phase 6 start)
11. **error-handling** — Error taxonomy, retry logic, logging, Application Insights (implement during Phase 6)

### Group 3: Assure & Ship (Phases 7–9)

7. **testing** — Unit, integration, E2E, UAT, and performance testing
8. **accessibility** — WCAG 2.2 AA audit, ARIA patterns, compliance report
9. **governance** — Documentation, runbooks, naming conventions, change logs

## Skill reference

Each skill lives in `agents/skills/[skill-name]/SKILL.md`. Read the relevant SKILL.md before starting that phase's work. Files marked *(stub)* exist as reference stubs — they are actionable but not yet fully expanded.

| Phase | Skill Folder | Purpose | Status |
|-------|-------------|---------|--------|
| 1 | `brainstorming` | Problem definition, personas, scope, success criteria, platform limitations | Ready |
| 2 | `architecture` | Component design, hosting model, ALM, security posture | Stub |
| 3 | `data-structure` | Data sources, ER diagrams, delegation, performance | Ready |
| 4 | `mock-up` | Working React/TS mockup components with realistic sample data | Ready |
| 5 | `connectors` | Connector selection, connection references, generated services | Stub |
| 6 | `implement-code` | Project init, Copilot prompt sequences, build, deploy | Ready |
| 7 | `testing` | Unit, integration, E2E, UAT, performance | Stub |
| 8 | `accessibility` | WCAG 2.2 AA audit, ARIA patterns, compliance | Stub |
| 9 | `governance` | Documentation, runbooks, naming conventions, change logs | Stub |
| 10 | `coding-standards` | ESLint, Prettier, TypeScript config, PR checklist | Stub |
| 11 | `error-handling` | Error taxonomy, retry logic, logging, App Insights | Stub |
| — | `power-apps-code-apps` | Master orchestrator — platform overview, full phase table | Ready |

## PAC CLI reference

Common `pac code` commands are documented in `agents/skills/pac-reference.md`.
Consult this before producing any CLI instructions to ensure accuracy.

## Key conventions

- Default framework is **React** with **TypeScript**
- Default component library is **Fluent UI React**
- Data models use the generated services pattern from `@microsoft/power-apps`
- **Never edit files in `generated/services/`** — these are owned by the CLI
- Connector plans always note whether connectors are standard or premium
- Use **connection references** (not raw connection IDs) for multi-environment ALM
- Architecture diagrams use **Mermaid** syntax
- All mockups must be working, renderable code — not static wireframes
- Every skill includes ready-to-paste **GitHub Copilot prompts**

## Platform limitations — know before you start

Surface these in the brainstorming phase so they shape scope:

- Code apps do **not** run on the Power Apps mobile app or Windows app
- **Excel Online (Business)** and **Excel Online (OneDrive)** connectors are not supported
- No `PowerBIIntegration` function — Power BI data integration is not available
- No SharePoint Forms integration
- No Power Platform Git integration (as of initial release)
- No Storage SAS IP restriction support
- If schema changes on a connector, there is no refresh command — delete and re-add the data source

## Getting started

Ask the developer: *"What app are we building? Tell me about the business problem and who will use it."*

Then read `agents/skills/brainstorming/SKILL.md` and begin the interview.
