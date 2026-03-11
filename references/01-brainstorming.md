# Phase 1: Brainstorming

## Purpose

Define what the app does, who it serves, and what "done" looks like — before any
technical decisions are made. A good brainstorm session prevents scope creep and
surfaces constraints that shape every later phase.

## Deliverable

A **Project Brief** document (Markdown or .docx) containing the sections below.

## Interview Questions

Work through these with the user. Don't dump them all at once — group them naturally
into the conversation flow.

### Problem & Context
- What business problem does this app solve?
- What process is manual or broken today?
- Is there an existing app (canvas, model-driven, legacy) being replaced? If so, what works and what doesn't?

### Users & Personas
- Who are the primary users? (role, technical comfort, device)
- Are there secondary user groups (managers, admins, approvers)?
- Approximate user count? (affects licensing and performance planning)
- Are external / guest users (Azure B2B) involved?

### Scope & Boundaries
- What are the 3–5 "must have" capabilities for v1?
- What is explicitly out of scope for v1?
- Are there regulatory or compliance requirements (GDPR, accessibility WCAG)?

### Success Criteria
- How will you measure whether the app is successful?
- What does the happy path look like end-to-end?
- Are there SLAs or performance expectations?

### Constraints
- Which Power Platform environment will host the app?
- Is code apps enabled on that environment?
- Are there Data Loss Prevention (DLP) policies that restrict connector grouping?
- Budget / licensing considerations?

## Output Template

```markdown
# Project Brief: [App Name]

## Problem Statement
[One paragraph describing the business problem]

## Target Users
| Persona | Role | Device | Frequency |
|---------|------|--------|-----------|
| | | | |

## Core Capabilities (v1)
1. 
2. 
3. 

## Out of Scope (v1)
- 

## Success Criteria
- 

## Known Constraints
- Environment: 
- DLP policies: 
- Licensing: 
- Timeline: 

## Open Questions
- 
```

## GitHub Copilot Prompt

Once the brief is complete, the user can use this prompt in Copilot Chat to
generate a README for the repo:

```
@workspace Generate a README.md for a Power Apps Code App based on the following
project brief: [paste brief]. Include sections for: Overview, Prerequisites,
Getting Started, Architecture, and Deployment.
```

## Transition to Phase 2

Once the project brief is agreed, read `references/02-architecture.md` and begin
designing the solution architecture.
