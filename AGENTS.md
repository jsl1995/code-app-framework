# Power Apps Code Apps — Agent Instructions

Guide the developer through building a Power Apps Code App from scratch. Understand what the app needs to do, who it's for, then work through each skill in sequence to produce all deliverables (mock-up, data structure, Dataverse setup, implementation plan, and working code).

## How it works

There are three phases with a natural conversation flow through each skill. Always start with Phase 1 unless the developer says otherwise.

### Phase 1: Understand & Design

Start by reading the brainstorming skill in `agents/skills/brainstorming/SKILL.md`. Interview the developer to understand the business problem, users, scope, and success criteria. Summarize what you find into a **Project Brief**.

Then work through these skills in order:

1. **brainstorming** — Define the problem, personas, scope, and success criteria
2. **data-structure** — Design data sources, entity schemas, ER diagrams, delegation rules
3. **mock-up** — Generate real, working React/TypeScript mockup components with sample data

### Phase 2: Build the Backend

4. **create-dataverse** — Set up the Dataverse environment, create tables, columns, relationships, security roles, and sample data

### Phase 3: Plan & Implement

5. **plan-code** — Architecture decisions, connector manifest, coding standards, error handling strategy, test plan
6. **implement-code** — Scaffold the project, add data sources, generate components with Copilot, build and deploy
7. **power-apps-code-apps** — Master reference for the Power Apps Code Apps platform, conventions, and full lifecycle overview

## Skill reference

Each skill lives in `agents/skills/[skill-name]/SKILL.md`. Read the relevant SKILL.md before starting that phase's work.

| Skill Folder | Purpose |
|-------------|---------|
| `brainstorming` | Problem definition, personas, scope, success criteria |
| `data-structure` | Data sources, ER diagrams, delegation, performance |
| `mock-up` | Working React/TS mockup components with realistic sample data |
| `create-dataverse` | Dataverse table creation, columns, relationships, security roles |
| `plan-code` | Architecture, connectors, standards, error handling, test strategy |
| `implement-code` | Project init, Copilot prompt sequences, build, deploy |
| `power-apps-code-apps` | Master orchestrator — platform overview, full phase table, conventions |

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
