# Power Apps Code Apps Framework

An agent skills framework for building [Power Apps Code Apps](https://learn.microsoft.com/en-us/power-apps/developer/code-apps/overview) using VS Code and GitHub Copilot. It provides structured guidance, templates, and ready-to-use Copilot prompts covering the full development lifecycle — from brainstorming through to deployment, governance, and handover.

## What are Code Apps?

Code Apps let you build custom single-page applications (React, Vue, vanilla TS) that run on the Power Platform managed platform. You write a standard web app in VS Code, and Power Platform handles authentication, connector access, DLP enforcement, and hosting. Your app gets access to 1,500+ Power Platform connectors callable directly from JavaScript — no auth code, no middleware, no custom APIs.

## What's in this framework?

Eleven skills covering the full lifecycle, organised into three groups.

### Group 1: Design

Do these before writing any application code.

| Phase | Skill | What it covers |
|-------|-------|----------------|
| 1 | [Brainstorming](agents/skills/brainstorming/SKILL.md) | Problem definition, personas, scope, success criteria, platform limitations |
| 2 | [Architecture](agents/skills/architecture/SKILL.md) | Solution design, hosting model, ALM strategy, security posture |
| 3 | [Data Structure](agents/skills/data-structure/SKILL.md) | Data source selection, ER diagrams, delegation, performance |
| 4 | [Mock-up](agents/skills/mock-up/SKILL.md) | Working React/TS mockup components with realistic sample data |
| 5 | [Connectors](agents/skills/connectors/SKILL.md) | Connector manifest, connection references, generated services guidance |

### Group 2: Build

| Phase | Skill | What it covers |
|-------|-------|----------------|
| 6 | [Implement Code](agents/skills/implement-code/SKILL.md) | Project init, data source discovery, Copilot prompt sequences, build, deploy |
| 10 | [Coding Standards](agents/skills/coding-standards/SKILL.md) | TypeScript strict, ESLint, Prettier, Husky, VS Code settings (set up at Phase 6 start) |
| 11 | [Error Handling](agents/skills/error-handling/SKILL.md) | Error taxonomy, retry logic, ErrorBoundary, logging, Application Insights |

### Group 3: Assure & Ship

| Phase | Skill | What it covers |
|-------|-------|----------------|
| 7 | [Testing](agents/skills/testing/SKILL.md) | Unit, integration, E2E, accessibility, and UAT strategy |
| 8 | [Accessibility](agents/skills/accessibility/SKILL.md) | WCAG 2.2 AA audit, ARIA patterns, axe-core tooling |
| 9 | [Governance](agents/skills/governance/SKILL.md) | Handover pack, ops runbook, naming conventions, changelog |

**Supporting references:**
- [`AGENTS.md`](AGENTS.md) — root orchestrator, read by Copilot agent mode first
- [`agents/skills/pac-reference.md`](agents/skills/pac-reference.md) — PAC CLI command cheatsheet
- [`agents/skills/power-apps-code-apps/SKILL.md`](agents/skills/power-apps-code-apps/SKILL.md) — master platform reference

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

## Prerequisites

- [VS Code](https://code.visualstudio.com/) with the [GitHub Copilot](https://marketplace.visualstudio.com/items?itemName=GitHub.copilot) and [GitHub Copilot Chat](https://marketplace.visualstudio.com/items?itemName=GitHub.copilot-chat) extensions
- [Node.js](https://nodejs.org/) (LTS version)
- [Power Apps CLI](https://learn.microsoft.com/en-us/power-platform/developer/cli/introduction) (`pac`) installed, **or** the npm-based CLI (`npx @microsoft/power-apps`) from v1.0.4+
- A Power Platform environment with the **Code Apps** feature enabled
- Power Apps Premium licences for all end users
- A GitHub account (for source control and Copilot)

## Getting started

### Step 1: Clone this framework

```bash
git clone https://github.com/jsl1995/code-app-framework.git
code code-app-framework
```

### Step 2: Start a new project

Open Copilot Chat in VS Code, switch to **Agent Mode** (`@workspace`), and start with:

```
@workspace I want to build a Power Apps Code App. Let's start from the beginning.
```

Copilot will read `AGENTS.md` and guide you through Phase 1 (Brainstorming).

### Step 3: Or jump to a specific phase

If you're mid-project, open the relevant `SKILL.md` directly and follow its instructions. Each skill is self-contained.

## Using with GitHub Copilot

Every skill includes ready-to-paste prompts for **Copilot Chat** and **Agent Mode**.

### Agent Mode (recommended)

Agent Mode (`@workspace`) gives Copilot access to your entire project, including `AGENTS.md`, skill files, and generated service files.

```
@workspace Create a React component for AccountListPage that:
- Fetches data from DataverseService using the generated service in /generated/services/
- Displays results in a Fluent UI DetailsList with sortable columns
- Includes a SearchBox filter
- Shows a Spinner during loading and a MessageBar on error
Use TypeScript, functional components with hooks.
```

### File references in prompts

Point Copilot at specific files for tighter context:

```
Using #file:generated/services/DataverseService.ts, create a custom hook
called useAccounts that fetches and caches the account list with loading,
error, and refetch states.
```

### Prompt sequence (implement-code skill)

The `implement-code` skill provides a structured prompt sequence for generating a full app:

1. App shell and routing — layout, navigation, error boundary
2. Dashboard page — metrics, recent items
3. List page — data table, filtering, pagination
4. Detail page — view, edit, delete
5. Create form — validation, lookups, submission

## Using with Claude Code

This framework also works as a skill set for [Claude Code](https://docs.anthropic.com/en/docs/claude-code). Clone this repo and ask Claude to work through any phase using the skill files as context.

## Project structure

```
code-app-framework/
├── AGENTS.md                              # Root orchestrator — Copilot reads this first
├── README.md                              # This file
└── agents/
    └── skills/
        ├── pac-reference.md               # PAC CLI command cheatsheet (all pac code commands)
        ├── brainstorming/SKILL.md         # Phase 1: problem definition & scoping
        ├── architecture/SKILL.md          # Phase 2: solution architecture & ALM
        ├── data-structure/SKILL.md        # Phase 3: data sources, ER diagrams, delegation
        ├── mock-up/SKILL.md               # Phase 4: working React/TS mockup generation
        ├── connectors/SKILL.md            # Phase 5: connector manifest & connection references
        ├── implement-code/SKILL.md        # Phase 6: build, Copilot prompts, deploy
        ├── testing/SKILL.md               # Phase 7: unit, integration, E2E, UAT
        ├── accessibility/SKILL.md         # Phase 8: WCAG 2.2 AA audit
        ├── governance/SKILL.md            # Phase 9: handover pack & runbook
        ├── coding-standards/SKILL.md      # Phase 10: ESLint, Prettier, TypeScript config
        ├── error-handling/SKILL.md        # Phase 11: retry logic, logging, App Insights
        ├── create-dataverse/SKILL.md      # Dataverse setup, tables, security roles
        ├── plan-code/SKILL.md             # Architecture, connectors, standards, test strategy
        └── power-apps-code-apps/SKILL.md  # Master platform reference & conventions
```

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
