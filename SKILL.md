---
name: powerapps-code-apps
description: >
  End-to-end skill for developing Power Apps Code Apps using VS Code and GitHub Copilot.
  Covers the full lifecycle: brainstorming, solution architecture, data architecture,
  UI mockups, connector & data source planning, scaffolding, and deployment.
  Use this skill whenever the user mentions Power Apps code apps, pac code, code-first
  Power Apps, building SPAs on Power Platform, connecting to Power Platform connectors
  from JavaScript/TypeScript, or developing React/Vue apps that run in Power Platform.
  Also trigger when the user asks about the @microsoft/power-apps npm package,
  pac code init, pac code push, or building line-of-business web apps with
  Power Platform managed hosting. Even if the user just says "I want to build a
  custom web app that uses Dataverse" or "code-first app on Power Platform", use this skill.
---

# Power Apps Code Apps — Full Lifecycle Development Skill

This skill guides you through building Power Apps Code Apps in VS Code with GitHub Copilot.
It is organised into six phases, each backed by a dedicated reference file. Read the
relevant phase file before executing that phase's work.

## What are Code Apps?

Code Apps let developers build custom SPAs (React, Vue, plain HTML/TS) that run on the
Power Platform managed platform. You get full control over UI and logic while inheriting:

- Microsoft Entra authentication (no auth code to write)
- Access to 1,500+ Power Platform connectors callable from JS/TS
- Managed hosting, DLP, Conditional Access, sharing limits
- Simple deployment via `pac code push`

**Prerequisites**: VS Code, Node.js LTS, Power Apps CLI (or `@microsoft/power-apps` npm CLI v1.0.4+), GitHub Copilot extension, a Power Platform environment with code apps enabled, Power Apps Premium licences for end users.

## Workflow Overview

Work through these phases in order. Each phase has a dedicated reference file in `references/`.
Read the relevant file before starting that phase.

| Phase | Reference File | Purpose |
|-------|---------------|---------|
| 1. Brainstorming | `references/01-brainstorming.md` | Define the problem, users, scope, and success criteria |
| 2. Solution Architecture | `references/02-architecture.md` | Component design, hosting model, ALM, security posture |
| 3. Data Architecture | `references/03-data-architecture.md` | Data sources, schema, delegation, relationships |
| 4. UI Mockups | `references/04-ui-mockups.md` | Wireframes, component hierarchy, responsive layout |
| 5. Connectors & Data Sources | `references/05-connectors.md` | Connector selection, connection references, generated services |
| 6. Scaffolding & Build | `references/06-scaffolding.md` | Project init, Copilot prompts, build, test, deploy |
| 7. Testing & QA | `references/07-testing.md` | Unit, integration, E2E, UAT, and performance testing |
| 8. Accessibility & Compliance | `references/08-accessibility.md` | WCAG 2.2 AA audit, ARIA patterns, compliance report |
| 9. Governance & Handover | `references/09-governance.md` | Documentation, runbooks, naming conventions, change logs |
| 10. Coding Standards | `references/10-coding-standards.md` | ESLint, Prettier, TypeScript config, PR review checklist |
| 11. Error Handling & Observability | `references/11-error-handling.md` | Error taxonomy, retry logic, logging, Application Insights |

## How to use this skill

1. **Identify where the user is in the lifecycle.** They may be starting fresh or jumping
   into a specific phase. Ask if unclear.
2. **Read the relevant reference file(s)** using the `view` tool before producing output.
3. **Produce the phase deliverable** as described in the reference file.
4. **When multiple phases are requested at once**, work through them sequentially, producing
   each deliverable before moving to the next.
5. **Use GitHub Copilot context**: throughout the skill, provide Copilot-ready prompts that
   the user can paste into VS Code agent mode or inline chat.

## Phase Groupings

Phases fall into three natural groups. Not every project needs every phase, but
the groupings help prioritise:

**Design (Phases 1–5)** — do these before writing any code.
Always do Phases 1–3. Phase 4 (UI) and 5 (Connectors) can overlap.

**Build (Phases 6, 10, 11)** — the active development phase.
Phase 10 (Coding Standards) should be set up at the start of Phase 6.
Phase 11 (Error Handling) is implemented during Phase 6 but has its own reference.

**Assure & Ship (Phases 7–9)** — quality, compliance, and handover.
Phase 7 (Testing) runs throughout build. Phases 8 and 9 gate the release.

## Key conventions

- All file outputs use TypeScript unless the user specifies JavaScript.
- Default framework is React; ask if the user prefers Vue or vanilla.
- Data models use the generated services pattern from `@microsoft/power-apps`.
- Connector plans always note whether connectors are standard or premium.
- Architecture diagrams use Mermaid syntax for portability.
