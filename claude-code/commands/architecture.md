# Phase 2: Architecture

Read `agents/skills/architecture/SKILL.md` for full guidance, then act directly.

## Steps

### 1. Read context
Read `UseCase.md` and `docs/project-brief.md` (if it exists). Extract the environment
URL, DLP policies, solution name, target environments, and ALM preferences.

Tell the developer:
> "[Phase 2/11: Architecture] Based on your use case I'll design the component
> architecture, ALM strategy, and security posture. [List any gaps to resolve first.]"

### 2. Ask only about gaps
Ask only about ALM and environment details not answered in UseCase.md:
- Environment chain URLs (Dev / Test / Prod)
- Branching strategy preference
- CI/CD tooling (GitHub Actions / Azure Pipelines / manual)
- Guest user requirements
- Conditional Access or MFA policies

### 3. Write the architecture document
Using the SKILL.md template, write `docs/architecture.md` directly. Include:
- **Mermaid component diagram** — three runtime layers (SPA → SDK → Platform) plus
  all planned data sources from UseCase.md
- **ALM decisions checklist** with environment topology
- **Connection references plan** — one per connector, named for the solution
- **Security checklist** — DLP compliance, authorisation, input validation
- **Risks table** — surface platform limitations not yet acknowledged

Use real environment URLs, solution name, and persona names from UseCase.md.

### 4. Confirm
Show the architecture document and ask:
> "Does this architecture match your environment setup? Any changes before we move
> to data structure?"

Wait for confirmation, then say:
> "Architecture saved to `docs/architecture.md`. Run `/data-structure` next."
