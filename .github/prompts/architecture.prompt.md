---
agent: 'agent'
description: 'Phase 2: Design the solution architecture, ALM strategy, and security posture'
tools: ['editFiles', 'createFile', 'readFile']
---

You are running Phase 2 of a Power Apps Code App build.

Read the full guidance in [architecture skill](../../agents/skills/architecture/SKILL.md).

## Your task

### Step 1: Read UseCase.md

Read `UseCase.md` from the project root. Extract the environment URL, DLP policies,
solution name, target environments, and ALM preferences.

Summarise what you found:
> "I've read your UseCase.md. For the architecture I have: [environment URL status, DLP info, ALM preferences]. I have [n] questions about the sections that need more detail."

### Step 2: Ask only about gaps

Ask only about ALM and environment information not answered in UseCase.md:
- Environment chain (Dev / Test / Prod URLs)
- Branching strategy preference
- CI/CD tooling (GitHub Actions / Azure Pipelines / manual)
- Guest user requirements
- Conditional Access or MFA policies

If UseCase.md already covers these, skip this step.

### Step 3: Generate the architecture deliverables

Using the architecture skill as a guide, produce:

1. A **Mermaid component diagram** showing the three runtime layers (SPA → SDK → Platform)
   and all planned data sources
2. An **ALM decisions checklist** with environment topology and connection reference plan
3. A **security checklist** covering DLP compliance, authorisation, and input validation
4. A **Solution Architecture Document** following the template in the skill file —
   save as `docs/architecture.md`

Use the real environment URL, solution name, and persona names from UseCase.md.

### Step 4: Confirm

Show the architecture document to the developer and ask:
> "Does this architecture match your environment setup and constraints? Any changes before we move to data structure?"

Wait for confirmation before finishing.

---

When confirmed, tell the developer:
> "Architecture saved to `docs/architecture.md`. Type `/data-structure` to design the data layer next."
