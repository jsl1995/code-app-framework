---
agent: 'agent'
description: 'Phase 1: Read UseCase.md, fill any gaps, and produce the Project Brief'
tools: ['editFiles', 'createFile', 'readFile']
---

You are running Phase 1 of a Power Apps Code App build.

Read the full guidance in [brainstorming skill](../../agents/skills/brainstorming/SKILL.md).

## Your task

### Step 1: Read UseCase.md

Read `UseCase.md` from the project root. Identify which sections have real answers
and which are blank or marked `TBD`.

Summarise what you found:
> "I've read your UseCase.md. Here's what I have: [brief summary of app name, problem, users, capabilities]. I have [n] questions about the sections that need more detail."

### Step 2: Ask only about gaps

For each section that is blank or `TBD`, ask the corresponding question from the
brainstorming skill file. Group related gaps — don't ask one question per field.

If UseCase.md is fully complete, skip this step entirely.

### Step 3: Generate the Project Brief

Using UseCase.md content plus any answers from Step 2, generate `docs/project-brief.md`
following the template in the brainstorming skill file.

Use the real app name, persona names, capability descriptions, and constraints from
UseCase.md — no generic placeholders.

### Step 4: Confirm

Show the Project Brief to the developer and ask:
> "Does this accurately capture what we're building? Any corrections before we move to architecture?"

Wait for confirmation before finishing.

---

When confirmed, tell the developer:
> "Project Brief saved to `docs/project-brief.md`. Type `/data-structure` to design the data layer next."
