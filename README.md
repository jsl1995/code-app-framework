# Power Apps Code Apps Framework

An agent skills framework for building [Power Apps Code Apps](https://learn.microsoft.com/en-us/power-apps/developer/code-apps/overview) using VS Code and GitHub Copilot. It provides structured guidance, templates, and ready-to-use Copilot prompts covering the full lifecycle from brainstorming through to deployment.

## What are Code Apps?

Code Apps let you build custom single-page applications (React, Vue, vanilla TS) that run on the Power Platform managed platform. You write a standard web app in VS Code, and Power Platform handles authentication, connector access, DLP enforcement, and hosting. Your app gets access to 1,500+ Power Platform connectors callable directly from JavaScript — no auth code, no middleware, no custom APIs.

## What's in this framework?

| Skill | Folder | What it covers |
|-------|--------|----------------|
| 1. Brainstorming | [`agents/skills/brainstorming`](agents/skills/brainstorming/SKILL.md) | Problem definition, personas, scope, success criteria |
| 2. Data Structure | [`agents/skills/data-structure`](agents/skills/data-structure/SKILL.md) | Data source selection, ER diagrams, delegation, performance |
| 3. Mock-up | [`agents/skills/mock-up`](agents/skills/mock-up/SKILL.md) | Working React/TS mockup components with realistic sample data |
| 4. Create Dataverse | [`agents/skills/create-dataverse`](agents/skills/create-dataverse/SKILL.md) | Dataverse table creation, columns, relationships, security roles |
| 5. Plan Code | [`agents/skills/plan-code`](agents/skills/plan-code/SKILL.md) | Architecture, connectors, coding standards, error handling, test strategy |
| 6. Implement Code | [`agents/skills/implement-code`](agents/skills/implement-code/SKILL.md) | Project init, Copilot prompt sequences, build, deploy |
| 7. Power Apps Code Apps | [`agents/skills/power-apps-code-apps`](agents/skills/power-apps-code-apps/SKILL.md) | Platform overview, full lifecycle conventions, master reference |

The [`AGENTS.md`](AGENTS.md) file at the root is the entry point for Copilot agent mode.

## Prerequisites

Before using this framework, make sure you have:

- [VS Code](https://code.visualstudio.com/) with the [GitHub Copilot](https://marketplace.visualstudio.com/items?itemName=GitHub.copilot) and [GitHub Copilot Chat](https://marketplace.visualstudio.com/items?itemName=GitHub.copilot-chat) extensions
- [Node.js](https://nodejs.org/) (LTS version)
- [Power Apps CLI](https://learn.microsoft.com/en-us/power-platform/developer/cli/introduction) (`pac`) installed and on your PATH
- A Power Platform environment with the **Code Apps** feature enabled
- Power Apps Premium licences for end users
- A GitHub account (for source control and Copilot)

## Getting started

### Step 1: Clone this framework

```bash
git clone https://github.com/jsl1995/code-app-framework.git
```

### Step 2: Open in VS Code

```bash
code code-app-framework
```

### Step 3: Start using the skills

Open the skill you need from the `agents/skills/` folder. Each `SKILL.md` is self-contained with templates, code examples, and Copilot prompts.

**Starting a new project?** Begin with `agents/skills/brainstorming/SKILL.md` and work through sequentially.

**Joining mid-flight?** Jump to the relevant skill — they work independently too.

## Using with GitHub Copilot

This framework is designed to work hand-in-hand with GitHub Copilot in VS Code. Every skill includes ready-to-paste prompts for both **Copilot Chat** and **Agent Mode**.

### Copilot Agent Mode (recommended)

Agent Mode (`@workspace`) gives Copilot access to your entire project. To use it:

1. Open Copilot Chat in VS Code
2. Start your prompt with `@workspace`
3. Copilot will scan your project files — including the `AGENTS.md` and skill files — and generate contextually accurate code

**Example:**
```
@workspace Create a React component for the AccountListPage that:
- Fetches data from DataverseService using the generated service in /generated/services/
- Displays results in a Fluent UI DetailsList with sortable columns
- Includes a SearchBox filter
- Shows a Spinner during loading and a MessageBar on error
Use TypeScript, functional components with hooks.
```

### Copilot Inline Chat

For quick edits to existing code, select code and press `Ctrl+I` / `Cmd+I`:

```
Add WCAG 2.2 AA accessibility attributes to this form: aria-required on
mandatory fields, aria-invalid on error states, and associate error
messages using aria-describedby.
```

### File references in prompts

Use `#file:` to point Copilot at specific files for context:

```
Using #file:generated/services/DataverseService.ts, create a custom hook
called useAccounts that fetches and caches the account list with loading,
error, and refetch states.
```

### Prompt sequence for a full build

The `implement-code` skill provides a structured prompt sequence that walks you through generating an entire app:

1. **App shell and routing** — layout, navigation, error boundary
2. **Dashboard page** — metrics, recent items
3. **List page** — data table, filtering, pagination
4. **Detail page** — view, edit, delete
5. **Create form** — validation, lookups, submission

## Using with Claude Code

This framework also works as a skill set for [Claude Code](https://docs.anthropic.com/en/docs/claude-code). Clone this repo into your project or reference it from your skill path, and ask Claude to help with any phase.

## Project structure

```
code-app-framework/
├── AGENTS.md                              # Root orchestrator — Copilot reads this first
├── README.md                              # This file
├── agents/
│   └── skills/
│       ├── brainstorming/
│       │   └── SKILL.md                   # Problem definition & scoping
│       ├── create-dataverse/
│       │   └── SKILL.md                   # Dataverse setup, tables, security
│       ├── data-structure/
│       │   └── SKILL.md                   # Data sources, ER diagrams, delegation
│       ├── implement-code/
│       │   └── SKILL.md                   # Build, Copilot prompts, deploy
│       ├── mock-up/
│       │   └── SKILL.md                   # Working React/TS mockup generation
│       ├── plan-code/
│       │   └── SKILL.md                   # Architecture, connectors, standards
│       └── power-apps-code-apps/
│           └── SKILL.md                   # Master reference & conventions
├── .vscode/                               # VS Code workspace settings
└── docs/
    └── assets/                            # Documentation assets
```

## Key resources

- [Power Apps Code Apps Overview](https://learn.microsoft.com/en-us/power-apps/developer/code-apps/overview)
- [Code Apps Architecture](https://learn.microsoft.com/en-us/power-apps/developer/code-apps/architecture)
- [Connect to Data](https://learn.microsoft.com/en-us/power-apps/developer/code-apps/how-to/connect-to-data)
- [Power Apps Code Apps GitHub](https://github.com/microsoft/PowerAppsCodeApps) — Samples and templates from Microsoft
- [GitHub Copilot Docs](https://docs.github.com/en/copilot)

## Contributing

Contributions welcome. Fork, branch (`feature/your-improvement`), and submit a PR.

## Licence

This framework is provided as-is for use in Power Apps Code App development projects. Use it, adapt it, share it.
