# Phase 1: Brainstorming

## Purpose

Validate the content of `UseCase.md`, fill any gaps through targeted questions, and
produce a confirmed Project Brief that all subsequent phases build on.

## Deliverable

A **Project Brief** saved as `docs/project-brief.md`, derived from UseCase.md and
confirmed by the developer before moving to Phase 2.

## How to run this phase

### Step 1: Read UseCase.md

Read `UseCase.md` from the project root. For each section, determine whether it is:
- **Complete** — has a real answer (not blank, not `TBD`, not a placeholder)
- **Partial** — has some content but needs clarification
- **Missing** — blank or `TBD`

### Step 2: Ask only about gaps

Ask the developer **only** about missing or partial sections. Group related gaps
into a single question where possible — do not fire a question per field.

If UseCase.md is fully complete, skip straight to Step 3.

### Gap questions by section

Use these only when the corresponding UseCase.md section is missing or unclear:

**Problem & context** *(if Problem Statement or Existing Solution is TBD)*
- What business problem does this app solve?
- What process is manual or broken today?
- Is there an existing app being replaced? If so, what works and what doesn't?

**Users & personas** *(if Target Users table is empty)*
- Who are the primary users? (role, technical comfort, device)
- Are there secondary user groups (managers, admins, approvers)?
- Approximate user count?
- Are external / guest users (Azure B2B) involved?

**Scope** *(if Core Capabilities or Out of Scope is TBD)*
- What are the 3–5 "must have" capabilities for v1?
- What is explicitly out of scope?

**Success criteria** *(if Success Criteria is TBD)*
- How will you measure whether the app is successful?
- What does the happy path look like end-to-end?
- Are there SLAs or performance expectations?

**Constraints** *(if Environment, DLP, Licensing, or Timeline is TBD)*
- Which Power Platform environment will host the app?
- Is code apps enabled on that environment?
- Are there DLP policies that restrict connector grouping?
- Budget / licensing considerations?
- Target go-live date?

**Platform limitations** *(if any limitation rows are not confirmed in UseCase.md)*
- Surface unacknowledged limitations and ask the developer to confirm they've accounted for them:
  - Desktop browser only — no mobile or Windows app
  - Excel Online connectors are not supported
  - No Power BI data integration
  - No SharePoint Forms integration

### Step 3: Generate the Project Brief

Using the confirmed content from UseCase.md plus any gap answers, produce
`docs/project-brief.md` using the template below.

Use the actual app name, persona names, capability descriptions, and constraint
details from UseCase.md — never use generic placeholders.

```markdown
# Project Brief: [App Name from UseCase.md]

## Problem Statement
[From UseCase.md — one paragraph describing the business problem]

## Existing Solution
[From UseCase.md — what is being replaced and what pain points it has]

## Target Users
| Persona | Role | Device | Frequency |
|---------|------|--------|-----------|
[Rows from UseCase.md Target Users table]

**Total users:** [from UseCase.md]
**Guest users:** [Yes / No from UseCase.md]

## Core Capabilities (v1)
[Numbered list from UseCase.md Core Capabilities]

## Out of Scope (v1)
[List from UseCase.md Out of Scope]

## Success Criteria
[From UseCase.md Success Criteria]

## Performance Expectations
[From UseCase.md]

## Compliance Requirements
[From UseCase.md]

## Data
- Preferred data source: [from UseCase.md]
- Existing data / migration: [from UseCase.md]
- Approximate row counts: [from UseCase.md]

## Known Constraints
- Environment: [from UseCase.md]
- Code Apps enabled: [from UseCase.md]
- DLP policies: [from UseCase.md]
- Solution name: [from UseCase.md]
- Target environments: [from UseCase.md]
- Licensing confirmed: [from UseCase.md]
- Timeline: [from UseCase.md]

## Platform Limitations — Confirmed
[List only limitations confirmed as acknowledged in UseCase.md, flag any still TBD]

## Open Questions
[Any unresolved items from UseCase.md Open Questions or raised during this phase]
```

### Step 4: Confirm before proceeding

Show the Project Brief to the developer and ask:
> "Does this Project Brief accurately capture what we're building? Any corrections
> before we move to architecture?"

Do not proceed to Phase 2 until the developer confirms.

## GitHub Copilot Prompt

Once the brief is confirmed, the user can use this prompt to generate a repo README:

```
@workspace Generate a README.md for a Power Apps Code App based on the project
brief in docs/project-brief.md. Include sections for: Overview, Prerequisites,
Getting Started, Architecture, and Deployment. Use the app name, persona names,
and capability descriptions from the brief — no generic placeholders.
```

## Transition to Phase 2

Once the Project Brief is confirmed, read `agents/skills/architecture/SKILL.md` and
design the solution architecture — using the environment URL, DLP info, and ALM
preferences from UseCase.md rather than asking again.
