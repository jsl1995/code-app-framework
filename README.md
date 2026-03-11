# Power Apps Code Apps Framework

An 11-phase development framework for building [Power Apps Code Apps](https://learn.microsoft.com/en-us/power-apps/developer/code-apps/overview) using VS Code and GitHub Copilot. It provides structured guidance, templates, and ready-to-use Copilot prompts covering the full lifecycle from brainstorming through to handover.

## What are Code Apps?

Code Apps let you build custom single-page applications (React, Vue, vanilla TS) that run on the Power Platform managed platform. You write a standard web app in VS Code, and Power Platform handles authentication, connector access, DLP enforcement, and hosting. Your app gets access to 1,500+ Power Platform connectors callable directly from JavaScript — no auth code, no middleware, no custom APIs.

## What's in this framework?

| Phase | File | What it covers |
|-------|------|----------------|
| 1. Brainstorming | [`references/01-brainstorming.md`](references/01-brainstorming.md) | Problem definition, personas, scope, success criteria |
| 2. Solution Architecture | [`references/02-architecture.md`](references/02-architecture.md) | Framework choice, state management, ALM, security model |
| 3. Data Architecture | [`references/03-data-architecture.md`](references/03-data-architecture.md) | Data source selection, ER diagrams, delegation, performance |
| 4. UI Mockups | [`references/04-ui-mockups.md`](references/04-ui-mockups.md) | Wireframes, component tree, responsive breakpoints |
| 5. Connectors & Data Sources | [`references/05-connectors.md`](references/05-connectors.md) | Connector manifest, DLP checks, `pac code add-data-source` commands |
| 6. Scaffolding & Build | [`references/06-scaffolding.md`](references/06-scaffolding.md) | Project init, Copilot prompt sequences, local dev, deployment |
| 7. Testing & QA | [`references/07-testing.md`](references/07-testing.md) | Unit, integration, E2E (Playwright), UAT scripts, performance |
| 8. Accessibility | [`references/08-accessibility.md`](references/08-accessibility.md) | WCAG 2.2 AA checklist, ARIA patterns, axe-core integration |
| 9. Governance & Handover | [`references/09-governance.md`](references/09-governance.md) | Documentation templates, runbooks, naming conventions |
| 10. Coding Standards | [`references/10-coding-standards.md`](references/10-coding-standards.md) | ESLint, Prettier, TypeScript config, PR review checklist |
| 11. Error Handling | [`references/11-error-handling.md`](references/11-error-handling.md) | Error taxonomy, retry logic, logging, Application Insights |

The orchestrator file [`SKILL.md`](SKILL.md) ties everything together and explains how the phases relate.

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

### Step 3: Start using the phases

Open the phase you need from the `references/` folder. Each file is self-contained with templates, code examples, and Copilot prompts.

**Starting a new project?** Begin with Phase 1 (`references/01-brainstorming.md`) and work through sequentially.

**Joining mid-flight?** Jump to the relevant phase — they're designed to work independently too.

## Using with GitHub Copilot

This framework is designed to work hand-in-hand with GitHub Copilot in VS Code. Every phase includes ready-to-paste prompts for both **Copilot Chat** and **Agent Mode**.

### How to use the Copilot prompts

1. **Open the reference file** for the phase you're working on
2. **Find the "GitHub Copilot Prompt" section** at the bottom of the file
3. **Copy the prompt** and paste it into Copilot Chat (`Ctrl+Shift+I` / `Cmd+Shift+I`)
4. **Customise the placeholders** (e.g., replace `[ServiceName]` with your actual service name)

### Copilot Agent Mode (recommended)

Agent Mode (`@workspace`) gives Copilot access to your entire project, which means it can read your generated service files and produce accurate code. To use it:

1. Open Copilot Chat in VS Code
2. Start your prompt with `@workspace`
3. Copilot will scan your project files and generate contextually accurate code

**Example — scaffolding a list page after adding connectors:**

```
@workspace Create a React component for the AccountListPage that:
- Fetches data from DataverseService using the generated service in /generated/services/
- Displays results in a Fluent UI DetailsList with sortable columns
- Includes a SearchBox filter
- Shows a Spinner during loading and a MessageBar on error
- Has a "New Account" CommandBarButton
Use TypeScript, functional components with hooks.
```

### Copilot Inline Chat

For quick edits to existing code, use inline chat (`Ctrl+I` / `Cmd+I`):

1. Select the code you want to modify
2. Press `Ctrl+I`
3. Describe what you want changed

**Example:**

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

The framework provides a structured prompt sequence in Phase 6 (`references/06-scaffolding.md`) that walks you through generating an entire app:

1. **App shell and routing** — layout, navigation, error boundary
2. **Dashboard page** — metrics, recent items
3. **List page** — data table, filtering, pagination
4. **Detail page** — view, edit, delete
5. **Create form** — validation, lookups, submission

Each prompt builds on the previous one, and Copilot uses the generated files from earlier steps as context.

## Using with Claude Code

This framework also works as a skill set for [Claude Code](https://docs.anthropic.com/en/docs/claude-code). The `SKILL.md` file acts as the orchestrator, and Claude will read the relevant `references/` files on demand.

To use it:

1. Clone this repo into your project or reference it from your Claude Code skill path
2. Ask Claude to help with any phase — e.g., *"Help me design the data architecture for an asset tracking code app using Dataverse"*
3. Claude will read the relevant reference file and produce the phase deliverable

## Phase groupings

Not every project needs all 11 phases. Here's how they group:

### Design (Phases 1–5)
Do these before writing code. Phases 1–3 are essential. Phases 4 and 5 can overlap.

### Build (Phases 6, 10, 11)
Active development. Set up Phase 10 (coding standards) at the start of Phase 6. Phase 11 (error handling) is implemented during build but has its own reference for the patterns.

### Assure & Ship (Phases 7–9)
Quality gates. Phase 7 (testing) runs throughout build. Phases 8 (accessibility) and 9 (governance) gate the release.

```
Design                          Build                    Assure & Ship
──────────────────────────────  ───────────────────────  ──────────────────
1. Brainstorming                6. Scaffolding & Build   7. Testing & QA
2. Solution Architecture       10. Coding Standards      8. Accessibility
3. Data Architecture           11. Error Handling        9. Governance &
4. UI Mockups                                               Handover
5. Connectors & Data Sources
```

## Project structure

```
code-app-framework/
├── README.md                              # This file
├── SKILL.md                               # Orchestrator — ties all phases together
└── references/
    ├── 01-brainstorming.md                # Problem definition & scoping
    ├── 02-architecture.md                 # Solution architecture & ADR
    ├── 03-data-architecture.md            # Data sources, ER diagrams, delegation
    ├── 04-ui-mockups.md                   # Wireframes & component hierarchy
    ├── 05-connectors.md                   # Connector manifest & DLP checks
    ├── 06-scaffolding.md                  # Build, Copilot prompts, deploy
    ├── 07-testing.md                      # Test strategy & implementation
    ├── 08-accessibility.md                # WCAG 2.2 AA compliance
    ├── 09-governance.md                   # Handover docs & naming conventions
    ├── 10-coding-standards.md             # Linting, formatting, PR standards
    └── 11-error-handling.md               # Error handling & observability
```

## Key resources

- [Power Apps Code Apps Overview](https://learn.microsoft.com/en-us/power-apps/developer/code-apps/overview) — Microsoft's official documentation
- [Code Apps Architecture](https://learn.microsoft.com/en-us/power-apps/developer/code-apps/architecture) — How the three runtime components fit together
- [Connect to Data](https://learn.microsoft.com/en-us/power-apps/developer/code-apps/how-to/connect-to-data) — Adding connectors and using generated services
- [Power Apps Code Apps GitHub](https://github.com/microsoft/PowerAppsCodeApps) — Samples and starter templates from Microsoft
- [GitHub Copilot Docs](https://docs.github.com/en/copilot) — Getting started with Copilot in VS Code

## Contributing

Contributions welcome. If you'd like to improve a phase or add a new one:

1. Fork this repo
2. Create a feature branch (`feature/your-improvement`)
3. Make your changes
4. Submit a PR with a description of what you've changed and why

## Licence

This framework is provided as-is for use in Power Apps Code App development projects. Use it, adapt it, share it.
