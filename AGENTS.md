# Power Apps Code Apps — Agent Instructions

Guide the developer through building a Power Apps Code App from scratch.

## First: read UseCase.md

**Before doing anything else**, read `UseCase.md` in the project root. It is the
single source of truth for this project. Extract every answer it contains, then:

- **Do not ask** the developer about anything already answered in UseCase.md
- **Do ask** about any field left blank, marked `TBD`, or genuinely ambiguous
- **Refer back** to UseCase.md throughout every phase — use its personas, data
  sources, environment URLs, constraints, and success criteria rather than inventing
  placeholders
- If UseCase.md does not exist or is completely blank, ask the developer to fill it
  in first: *"Before we start, please fill in `UseCase.md` with details about your
  use case. The more you add, the less I'll need to ask."*

## How it works

There are four groups with a natural conversation flow. Always start with Phase 1
unless the developer says otherwise. At each phase, read the relevant SKILL.md before
producing any output.

### Group 1: Design (Phases 1–5)

Do all of these before writing any application code.

1. **brainstorming** — Validate the UseCase.md content, fill gaps, produce the Project Brief
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

Each skill lives in `agents/skills/[skill-name]/SKILL.md`. Read the relevant SKILL.md
before starting that phase's work. Files marked *(stub)* are actionable but not yet
fully expanded.

| Phase | Skill Folder | Purpose | Status |
|-------|-------------|---------|--------|
| 1 | `brainstorming` | Validate UseCase.md, fill gaps, produce Project Brief | Ready |
| 2 | `architecture` | Component design, hosting model, ALM, security posture | Ready |
| 3 | `data-structure` | Data sources, ER diagrams, delegation, performance | Ready |
| 4 | `mock-up` | Working React/TS mockup components with realistic sample data | Ready |
| 5 | `connectors` | Connector selection, connection references, generated services | Ready |
| 6 | `implement-code` | Project init, Copilot prompt sequences, build, deploy | Ready |
| 7 | `testing` | Unit, integration, E2E, UAT, performance | Ready |
| 8 | `accessibility` | WCAG 2.2 AA audit, ARIA patterns, compliance | Ready |
| 9 | `governance` | Documentation, runbooks, naming conventions, change logs | Ready |
| 10 | `coding-standards` | ESLint, Prettier, TypeScript config, PR checklist | Ready |
| 11 | `error-handling` | Error taxonomy, retry logic, logging, App Insights | Ready |
| — | `power-apps-code-apps` | Master orchestrator — platform overview, full phase table | Ready |

## Entry points

This framework has two entry points — both read the same skill files.

| Entry point | File | How to use |
|-------------|------|-----------|
| **GitHub Copilot Agent Mode** | `AGENTS.md` (this file) | Open Copilot Chat → Agent Mode (`@workspace`) → `@workspace Let's build this code app` |
| **VS Code custom agent** | `.github/agents/code-app-builder.agent.md` | Open Copilot Chat → select `code-app-builder` agent → use slash commands |

### VS Code slash commands (code-app-builder agent)

When using the VS Code custom agent, these slash commands are available:

| Command | Phase | Description |
|---------|-------|-------------|
| `/brainstorming` | 1 | Validate UseCase.md and produce the Project Brief |
| `/data-structure` | 3 | Design data sources, ER diagrams, and delegation rules |
| `/mock-up` | 4 | Generate working React/TS mockup components |
| `/create-dataverse` | 6a | Set up Dataverse tables, columns, and security roles |
| `/plan-code` | 7 | Lock in technical decisions and produce the Implementation Plan |
| `/implement-code` | 8 | Scaffold, connect data sources, generate components, deploy |

> Phases 2 (Architecture) and 5 (Connectors) are covered by the Agent Mode flow above.
> Use `/architecture` or `/connectors` prompts directly from `.github/prompts/` in Agent Mode.

## Phase numbering reference

The Agent Mode flow (this file) uses an 11-phase model. The VS Code `code-app-builder` agent uses a condensed 8-phase model. Here is how they map:

| AGENTS.md phase | code-app-builder phase | Skill |
|----------------|----------------------|-------|
| 1 | 1 | brainstorming |
| 2 | 2 | architecture |
| 3 | 3 | data-structure |
| 4 | 4 | mock-up |
| 5 | 5 | connectors |
| 6 | 8 | implement-code |
| 7 | — | testing *(post-deployment)* |
| 8 | — | accessibility *(post-deployment)* |
| 9 | — | governance *(post-deployment)* |
| 10 | 7 (part) | coding-standards *(set up at build start)* |
| 11 | 7 (part) | error-handling *(implemented during build)* |
| — | 6 | create-dataverse *(Dataverse-specific setup)* |
| — | 7 | plan-code *(technical planning before build)* |

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
- **Always use names, entities, and personas from UseCase.md** — never use generic
  placeholders like `[EntityName]` or `[UserRole]` after Phase 1

## Platform limitations — know before you start

Surface any of these not yet acknowledged in UseCase.md:

- Code apps do **not** run on the Power Apps mobile app or Windows app
- **Excel Online (Business)** and **Excel Online (OneDrive)** connectors are not supported
- No `PowerBIIntegration` function — Power BI data integration is not available
- No SharePoint Forms integration
- No Power Platform Git integration (as of initial release)
- No Storage SAS IP restriction support
- If schema changes on a connector, there is no refresh command — delete and re-add the data source

## Getting started

1. Read `UseCase.md`
2. Identify what is answered and what is missing
3. Ask only about what is missing
4. Then read `agents/skills/brainstorming/SKILL.md` and produce the Project Brief
