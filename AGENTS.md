# Power Apps Code Apps — Agent Instructions

Guide the developer through building a Power Apps Code App from scratch. Understand what the app needs to do, who it's for, then work through each skill in sequence to produce all deliverables (mock-up, data structure, connectors, implementation, testing, and handover).

## How it works

There are three phases — **Design**, **Build**, and **Assure & Ship** — with a natural conversation flow through each skill. Always start with Phase 1 unless the developer says otherwise.

### Phase 1: Understand & Design

Start by reading the brainstorming skill in `agents/skills/brainstorming/SKILL.md`. Interview the developer to understand the business problem, users, scope, and success criteria. Summarize what you learn into a **Project Brief**.

Then work through these skills in order:

1. **brainstorming** — Define the problem, personas, scope, and success criteria
2. **architecture** — Choose framework, state management, ALM strategy, security model
3. **data-structure** — Design data sources, entity schemas, ER diagrams, delegation rules
4. **mock-up** — Generate real, working React/TypeScript mockup components with sample data
5. **connectors** — Map data requirements to Power Platform connectors, verify DLP compliance

### Phase 2: Build

Before writing any production code, set up standards:

6. **coding-standards** — Configure ESLint, Prettier, TypeScript, VS Code settings, git hooks
7. **implement-code** — Scaffold the project, add data sources, generate components with Copilot
8. **error-handling** — Implement error boundaries, connector wrappers with retry, logging

### Phase 3: Assure & Ship

9. **testing** — Unit tests, integration tests, E2E with Playwright, UAT scripts
10. **accessibility** — WCAG 2.2 AA audit, ARIA patterns, axe-core integration
11. **governance** — Documentation, runbooks, naming conventions, handover pack

## Skill reference

Each skill lives in `agents/skills/[skill-name]/SKILL.md`. Read the relevant SKILL.md before starting that phase's work. The orchestrator skill in `agents/skills/power-apps-code-apps/SKILL.md` contains the full phase table and conventions.

| Skill Folder | Purpose |
|-------------|---------|
| `brainstorming` | Problem definition, personas, scope, success criteria |
| `architecture` | Framework choice, state management, ALM, security model |
| `data-structure` | Data sources, ER diagrams, delegation, performance |
| `mock-up` | Working React/TS mockup components with realistic sample data |
| `connectors` | Connector manifest, DLP checks, pac code add-data-source commands |
| `coding-standards` | ESLint, Prettier, TypeScript config, PR review checklist |
| `implement-code` | Project init, Copilot prompt sequences, build, deploy |
| `error-handling` | Error taxonomy, retry logic, logging, Application Insights |
| `testing` | Unit, integration, E2E, UAT, performance testing |
| `accessibility` | WCAG 2.2 AA checklist, ARIA patterns, compliance report |
| `governance` | Documentation templates, runbooks, naming conventions, handover |
| `power-apps-code-apps` | Master orchestrator — full phase table and conventions |

## Key conventions

- Default framework is **React** with **TypeScript**
- Default component library is **Fluent UI React**
- Data models use the generated services pattern from `@microsoft/power-apps`
- Connector plans always note whether connectors are standard or premium
- Architecture diagrams use **Mermaid** syntax
- All mockups must be working, renderable code — not static wireframes
- Every skill includes ready-to-paste **GitHub Copilot prompts**

## Getting started

Ask the developer: *"What app are we building? Tell me about the business problem and who will use it."*

Then read `agents/skills/brainstorming/SKILL.md` and begin the interview.
