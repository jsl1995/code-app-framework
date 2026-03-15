# Power Apps Code Apps Framework

An agent skills framework for building [Power Apps Code Apps](https://learn.microsoft.com/en-us/power-apps/developer/code-apps/overview) using VS Code and GitHub Copilot. It provides structured guidance, templates, and ready-to-use Copilot prompts covering the full development lifecycle — from brainstorming through to deployment, governance, and handover.

## What are Code Apps?

Code Apps let you build custom single-page applications (React, Vue, vanilla TS) that run on the Power Platform managed platform. You write a standard web app in VS Code, and Power Platform handles authentication, connector access, DLP enforcement, and hosting. Your app gets access to 1,500+ Power Platform connectors callable directly from JavaScript — no auth code, no middleware, no custom APIs.

## How it works

The framework is driven by a single file: **`UseCase.md`**.

Fill in `UseCase.md` with details about your project before starting. The agent reads it at the beginning of every phase and uses it as the source of truth — it will only ask you questions about sections you've left blank. The more you fill in upfront, the less back-and-forth you'll need.

```
1. Fill in UseCase.md
       ↓
2. Open Copilot Agent Mode (@workspace)
       ↓
3. Agent reads UseCase.md, asks only about gaps
       ↓
4. Agent drives all 11 phases using your answers
```

## Getting started

### Step 1: Clone this framework

```bash
git clone https://github.com/jsl1995/code-app-framework.git
code code-app-framework
```

### Step 2: Fill in UseCase.md

Open `UseCase.md` and fill in as many sections as you can. If you want to see what
a complete `UseCase.md` looks like, refer to `UseCase.example.md` — it shows a fully
filled-in Asset Tracker app so you can understand the expected level of detail.

- What business problem are you solving?
- Who are the users (persona, role, device)?
- What are the v1 must-have capabilities?
- What data source do you want to use?
- What's your Power Platform environment URL?
- Are DLP policies in place?

The more you fill in, the fewer questions the agent will ask.

### Step 3: Start the agent

Open Copilot Chat in VS Code, switch to **Agent Mode** (`@workspace`), and say:

```
@workspace Let's build this code app.
```

The agent reads `AGENTS.md` and `UseCase.md`, summarises what it found, asks only about gaps, and guides you through all phases — using your real app name, entities, and personas throughout.

---

## What's in this framework?

Eleven skills covering the full lifecycle, organised into three groups.

### Group 1: Design

Do these before writing any application code.

| Phase | Skill | What it covers |
|-------|-------|----------------|
| 1 | [Brainstorming](agents/skills/brainstorming/SKILL.md) | Validate UseCase.md, fill gaps, produce Project Brief |
| 2 | [Architecture](agents/skills/architecture/SKILL.md) | Solution design, hosting model, ALM strategy, security posture |
| 3 | [Data Structure](agents/skills/data-structure/SKILL.md) | Data source selection, ER diagrams, delegation, performance |
| 4 | [Mock-up](agents/skills/mock-up/SKILL.md) | Working React/TS mockup components with realistic sample data |
| 5 | [Connectors](agents/skills/connectors/SKILL.md) | Connector manifest, connection references, generated services guidance |

### Group 2: Build

| Phase | Skill | What it covers |
|-------|-------|----------------|
| 6 | [Implement Code](agents/skills/implement-code/SKILL.md) | Project init, data source discovery, Copilot prompt sequences, build, deploy |
| 10 | [Coding Standards](agents/skills/coding-standards/SKILL.md) | TypeScript strict, ESLint, Prettier, Husky, VS Code settings |
| 11 | [Error Handling](agents/skills/error-handling/SKILL.md) | Error taxonomy, retry logic, ErrorBoundary, logging, Application Insights |

### Group 3: Assure & Ship

| Phase | Skill | What it covers |
|-------|-------|----------------|
| 7 | [Testing](agents/skills/testing/SKILL.md) | Unit, integration, E2E, accessibility, and UAT strategy |
| 8 | [Accessibility](agents/skills/accessibility/SKILL.md) | WCAG 2.2 AA audit, ARIA patterns, axe-core tooling |
| 9 | [Governance](agents/skills/governance/SKILL.md) | Handover pack, ops runbook, naming conventions, changelog |

**Supporting references:**
- [`AGENTS.md`](AGENTS.md) — root orchestrator, read by Copilot agent mode first
- [`UseCase.md`](UseCase.md) — your project's source of truth (fill this in first)
- [`agents/skills/pac-reference.md`](agents/skills/pac-reference.md) — PAC CLI command cheatsheet
- [`agents/skills/power-apps-code-apps/SKILL.md`](agents/skills/power-apps-code-apps/SKILL.md) — master platform reference

---

## Platform limitations

Know these before scoping a project — they are hard constraints, not workarounds:

| Limitation | Impact |
|-----------|--------|
| No Power Apps mobile or Windows app support | Desktop browser only |
| Excel Online (Business) and Excel Online (OneDrive) not supported | Cannot use Excel as a data source |
| No Power BI data integration | Embed via iframe if needed |
| No SharePoint Forms integration | Cannot replace SharePoint list forms |
| No Power Platform Git integration (initial release) | ALM via solution export/import only |
| Connector schema changes require delete-and-re-add | No refresh command exists |

These are also listed in `UseCase.md` — confirm them there as part of project setup.

---

## Prerequisites

- [VS Code](https://code.visualstudio.com/) with the [GitHub Copilot](https://marketplace.visualstudio.com/items?itemName=GitHub.copilot) and [GitHub Copilot Chat](https://marketplace.visualstudio.com/items?itemName=GitHub.copilot-chat) extensions
- [Node.js](https://nodejs.org/) (LTS version)
- [Power Apps CLI](https://learn.microsoft.com/en-us/power-platform/developer/cli/introduction) (`pac`) installed, **or** the npm-based CLI (`npx @microsoft/power-apps`) from v1.0.4+
- A Power Platform environment with the **Code Apps** feature enabled
- Power Apps Premium licences for all end users
- A GitHub account (for source control and Copilot)

---

## Which AI tool are you using?

The framework supports two AI tools. Both read the same skill files in `agents/skills/`.

| | GitHub Copilot | Claude Code |
|---|---|---|
| **Root config** | `AGENTS.md` (auto-detected) | `claude-code/CLAUDE.md` → copy to project root |
| **Slash commands** | `.github/prompts/*.prompt.md` | `claude-code/commands/` → copy to `.claude/commands/` |
| **How it works** | Generates prompts for you to run | Reads, writes, and runs commands autonomously |
| **Setup** | Clone and go | Copy two files/folders to project root |

---

## Using with Claude Code

### Setup

After cloning, copy the Claude Code files into place:

```bash
cp claude-code/CLAUDE.md CLAUDE.md
cp -r claude-code/commands .claude/commands
```

### Starting a session

Open Claude Code in the project root and say:

```
Let's build this code app.
```

Claude Code reads `CLAUDE.md` and `UseCase.md`, summarises what it found, asks only
about gaps, and works through each phase **autonomously** — reading files, writing
deliverables, and running PAC CLI and npm commands directly without copy-paste.

### Slash commands (Claude Code)

| Command | Phase | What Claude Code does |
|---------|-------|-----------------------|
| `/brainstorming` | 1 | Reads UseCase.md → writes `docs/project-brief.md` |
| `/architecture` | 2 | Designs architecture → writes `docs/architecture.md` |
| `/data-structure` | 3 | Designs schema → writes `docs/data-architecture.md` |
| `/mock-up` | 4 | Generates `.tsx` mockup files in `src/mockups/` |
| `/connectors` | 5 | Builds manifest → writes `docs/connector-manifest.md` |
| `/implement-code` | 6 | Scaffolds project, runs PAC CLI, converts mockups, deploys |
| `/testing` | 7 | Installs toolchain, writes tests, runs coverage |
| `/accessibility` | 8 | Runs axe, fixes violations, writes audit report |
| `/governance` | 9 | Writes README, runbook, app catalogue, CHANGELOG |
| `/coding-standards` | 10 | Writes ESLint/Prettier/tsconfig, installs Husky |
| `/error-handling` | 11 | Writes ErrorBoundary, retry logic, App Insights |
| `/create-dataverse` | — | Creates tables, columns, security roles via PAC CLI |
| `/plan-code` | — | Locks in technical decisions → writes implementation plan |

---

## Using with GitHub Copilot

Every skill includes ready-to-paste prompts for **Copilot Chat** and **Agent Mode**. There are two entry points — both read the same skill files.

### Agent Mode (recommended)

Agent Mode (`@workspace`) gives Copilot access to your entire project, including `AGENTS.md`, `UseCase.md`, skill files, and generated service files. The agent will use your real use case context rather than asking you to fill in placeholders.

```
@workspace Let's build this code app.
```

### VS Code custom agent + slash commands

The framework also ships with a custom VS Code Copilot agent (`code-app-builder`) defined in `.github/agents/code-app-builder.agent.md`. When Copilot detects this file, you can select `code-app-builder` from the agent picker and use these slash commands:

| Command | What it does |
|---------|-------------|
| `/brainstorming` | Phase 1 — validate UseCase.md and produce the Project Brief |
| `/data-structure` | Phase 3 — design data sources and ER diagrams |
| `/mock-up` | Phase 4 — generate working React/TS mockup components |
| `/create-dataverse` | Set up Dataverse tables, columns, and security roles |
| `/plan-code` | Technical planning — lock in architecture, standards, test strategy |
| `/implement-code` | Phase 6/8 — scaffold, connect data sources, generate components, deploy |
| `/architecture` | Phase 2 — solution architecture, ALM, and security posture |
| `/connectors` | Phase 5 — connector manifest and connection references |

### File references in prompts

Point Copilot at specific files for tighter context:

```
Using #file:generated/services/DataverseService.ts and the entity names in
#file:UseCase.md, create a custom hook called useBookings that fetches and
caches the booking list with loading, error, and refetch states.
```

### Prompt sequence (implement-code skill)

The `implement-code` skill provides a structured prompt sequence for generating a full app — each prompt uses the real component names, service files, and entity names from your UseCase.md.

## Using with Claude Code

This framework also works as a skill set for [Claude Code](https://docs.anthropic.com/en/docs/claude-code). Clone this repo, fill in `UseCase.md`, and ask Claude to work through any phase using the skill files as context.

---

## Project structure

```
code-app-framework/
├── AGENTS.md                              # GitHub Copilot root orchestrator
├── UseCase.md                             # Your project's source of truth — fill this in first
├── UseCase.example.md                     # Fully filled example (Asset Tracker app)
├── README.md                              # This file
├── CHANGELOG.md                           # Framework version history
│
├── claude-code/                           # Claude Code entry point
│   ├── CLAUDE.md                          # → copy to project root as CLAUDE.md
│   └── commands/                          # → copy to .claude/commands/
│       ├── brainstorming.md               # /brainstorming
│       ├── architecture.md                # /architecture
│       ├── data-structure.md              # /data-structure
│       ├── mock-up.md                     # /mock-up
│       ├── connectors.md                  # /connectors
│       ├── implement-code.md              # /implement-code
│       ├── testing.md                     # /testing
│       ├── accessibility.md               # /accessibility
│       ├── governance.md                  # /governance
│       ├── coding-standards.md            # /coding-standards
│       ├── error-handling.md              # /error-handling
│       ├── create-dataverse.md            # /create-dataverse
│       └── plan-code.md                   # /plan-code
│
├── .github/                               # GitHub Copilot entry point
│   ├── agents/
│   │   └── code-app-builder.agent.md      # VS Code custom agent definition
│   └── prompts/
│       ├── brainstorming.prompt.md        # /brainstorming
│       ├── architecture.prompt.md         # /architecture
│       ├── data-structure.prompt.md       # /data-structure
│       ├── mock-up.prompt.md              # /mock-up
│       ├── connectors.prompt.md           # /connectors
│       ├── create-dataverse.prompt.md     # /create-dataverse
│       ├── plan-code.prompt.md            # /plan-code
│       └── implement-code.prompt.md       # /implement-code
│
└── agents/                                # Shared skill files (used by both AI tools)
    └── skills/
        ├── pac-reference.md               # PAC CLI command cheatsheet
        ├── brainstorming/SKILL.md         # Phase 1
        ├── architecture/SKILL.md          # Phase 2
        ├── data-structure/SKILL.md        # Phase 3
        ├── mock-up/SKILL.md               # Phase 4
        ├── connectors/SKILL.md            # Phase 5
        ├── implement-code/SKILL.md        # Phase 6
        ├── testing/SKILL.md               # Phase 7
        ├── accessibility/SKILL.md         # Phase 8
        ├── governance/SKILL.md            # Phase 9
        ├── coding-standards/SKILL.md      # Phase 10
        ├── error-handling/SKILL.md        # Phase 11
        ├── create-dataverse/SKILL.md      # Dataverse setup
        ├── plan-code/SKILL.md             # Technical planning
        └── power-apps-code-apps/SKILL.md  # Master platform reference
```

---

## Key resources

- [Power Apps Code Apps Overview](https://learn.microsoft.com/en-us/power-apps/developer/code-apps/overview)
- [Code Apps Architecture](https://learn.microsoft.com/en-us/power-apps/developer/code-apps/architecture)
- [Connect to Data](https://learn.microsoft.com/en-us/power-apps/developer/code-apps/how-to/connect-to-data)
- [PAC CLI Reference](https://learn.microsoft.com/en-us/power-platform/developer/cli/reference/code)
- [Power Apps Code Apps GitHub](https://github.com/microsoft/PowerAppsCodeApps) — samples and templates from Microsoft
- [GitHub Copilot Docs](https://docs.github.com/en/copilot)

## Contributing

Contributions welcome. Fork, branch (`feature/your-improvement`), and submit a PR.

## Licence

This framework is provided as-is for use in Power Apps Code App development projects. Use it, adapt it, share it.
