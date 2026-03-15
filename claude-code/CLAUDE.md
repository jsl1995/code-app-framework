# Power Apps Code Apps — Claude Code Instructions

Guide the developer through building a Power Apps Code App from scratch.

> **Claude Code users:** Copy this file to your project root as `CLAUDE.md` and copy
> `claude-code/commands/` to `.claude/commands/` to enable slash commands.

---

## First: read UseCase.md

**Before doing anything else**, read `UseCase.md` in the project root. It is the
single source of truth for this project.

- **Do not ask** about anything already answered in UseCase.md
- **Do ask** about fields left blank, marked `TBD`, or genuinely ambiguous
- **Refer back** to UseCase.md throughout every phase — use its personas, data
  sources, environment URLs, constraints, and success criteria
- If UseCase.md does not exist or is completely blank, say:
  *"Before we start, please fill in `UseCase.md` with details about your use case.
  See `UseCase.example.md` for a fully completed example."*

---

## How it works

There are eleven phases across three groups. Always start with Phase 1 unless told
otherwise. At each phase, **read the relevant SKILL.md** before producing any output,
then act directly — read files, write deliverables, run commands.

### Group 1: Design (Phases 1–5)

Do all of these before writing any application code.

1. **brainstorming** — Validate UseCase.md, fill gaps, write `docs/project-brief.md`
2. **architecture** — Solution design, ALM strategy, security posture, write `docs/architecture.md`
3. **data-structure** — Data sources, schemas, ER diagrams, write `docs/data-architecture.md`
4. **mock-up** — Working React/TypeScript mockup components in `src/mockups/`
5. **connectors** — Connector manifest, connection references, write `docs/connector-manifest.md`

### Group 2: Build (Phases 6, 10, 11)

6. **implement-code** — Scaffold the project, add data sources, build and deploy
10. **coding-standards** — ESLint, Prettier, TypeScript config, Husky *(set up at Phase 6 start)*
11. **error-handling** — ErrorBoundary, retry logic, Application Insights *(implement during Phase 6)*

### Group 3: Assure & Ship (Phases 7–9)

7. **testing** — Unit, integration, E2E, accessibility, and UAT
8. **accessibility** — WCAG 2.2 AA audit, ARIA patterns, axe-core
9. **governance** — Handover pack, ops runbook, naming conventions

---

## Skill reference

Each skill lives in `agents/skills/[skill-name]/SKILL.md` relative to the framework root.
Read the relevant SKILL.md before starting each phase.

| Phase | Skill folder | Status |
|-------|-------------|--------|
| 1 | `brainstorming` | Ready |
| 2 | `architecture` | Ready |
| 3 | `data-structure` | Ready |
| 4 | `mock-up` | Ready |
| 5 | `connectors` | Ready |
| 6 | `implement-code` | Ready |
| 7 | `testing` | Ready |
| 8 | `accessibility` | Ready |
| 9 | `governance` | Ready |
| 10 | `coding-standards` | Ready |
| 11 | `error-handling` | Ready |
| — | `create-dataverse` | Supplemental |
| — | `plan-code` | Supplemental |
| — | `pac-reference.md` | PAC CLI cheatsheet |

---

## Claude Code conventions

Claude Code can act directly — use these capabilities throughout every phase:

- **Read files** with the Read tool — do not ask the developer to open files
- **Write deliverables** with the Write/Edit tools — create `docs/`, config files, and
  component files directly without asking the developer to copy-paste
- **Run PAC CLI and npm commands** with the Bash tool — scaffold projects, add data
  sources, build, and deploy without asking the developer to run commands manually
- **Search the codebase** with Grep/Glob — find generated service files, component
  names, and types automatically
- **One phase at a time** — complete and confirm each phase before moving to the next
- **Show progress** — indicate the current phase at the start of each response:
  `[Phase 3/11: Data Structure]`

---

## Slash commands

When `claude-code/commands/` has been copied to `.claude/commands/`, these slash
commands are available:

| Command | Phase | What it does |
|---------|-------|-------------|
| `/brainstorming` | 1 | Validate UseCase.md and write the Project Brief |
| `/architecture` | 2 | Design solution architecture and write `docs/architecture.md` |
| `/data-structure` | 3 | Design data layer and write `docs/data-architecture.md` |
| `/mock-up` | 4 | Generate working React/TS mockup components |
| `/connectors` | 5 | Build connector manifest and write `docs/connector-manifest.md` |
| `/implement-code` | 6 | Scaffold, connect data sources, build, and deploy |
| `/testing` | 7 | Set up test toolchain and generate test files |
| `/accessibility` | 8 | Run accessibility audit and produce compliance report |
| `/governance` | 9 | Produce handover pack and ops runbook |
| `/coding-standards` | 10 | Generate ESLint, Prettier, tsconfig, and Husky config |
| `/error-handling` | 11 | Implement ErrorBoundary, retry logic, and App Insights |
| `/create-dataverse` | — | Set up Dataverse tables, columns, and security roles |
| `/plan-code` | — | Produce the Technical Implementation Plan |

---

## Key conventions

- Default framework: **React** with **TypeScript**
- Default component library: **Fluent UI React v9**
- Generated services pattern from `@microsoft/power-apps` — **never edit `generated/services/`**
- Always use **connection references** (not raw connection IDs) for multi-environment ALM
- Architecture and data diagrams use **Mermaid** syntax (renders in GitHub and VS Code)
- All mockups must be **working, renderable `.tsx` files** — not static wireframes
- After Phase 1, always use **real names** from UseCase.md — never generic placeholders

---

## Platform limitations — know before you start

Surface any of these not yet acknowledged in UseCase.md:

- Code apps do **not** run on the Power Apps mobile app or Windows app
- **Excel Online (Business)** and **Excel Online (OneDrive)** connectors are not supported
- No `PowerBIIntegration` function — Power BI data integration is not available
- No SharePoint Forms integration
- No Power Platform Git integration (as of initial release)
- If connector schema changes, there is no refresh — delete and re-add the data source
- All end users require **Power Apps Premium** licences

---

## Getting started

1. Read `UseCase.md` — identify what is answered and what is missing
2. Ask only about missing sections (or jump straight to the Project Brief if complete)
3. Read `agents/skills/brainstorming/SKILL.md` and write `docs/project-brief.md`
4. Confirm with the developer, then move to Phase 2
