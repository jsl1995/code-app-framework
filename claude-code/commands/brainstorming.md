# Phase 1: Brainstorming

Read `agents/skills/brainstorming/SKILL.md` for full guidance, then act directly.

## Steps

### 1. Read context
Read `UseCase.md` and `UseCase.example.md` (if present). For every section in
UseCase.md, classify it as Complete / Partial / Missing.

### 2. Summarise findings
Tell the developer:
> "[Phase 1/11: Brainstorming] I've read your UseCase.md. Here's what I have:
> [brief summary — app name, problem, users, key capabilities].
> I have [n] questions about sections that need more detail."

If UseCase.md is fully complete, skip to step 4.

### 3. Ask only about gaps
Ask grouped questions for missing sections — one question per topic area, not one
per field. Use the gap questions defined in `agents/skills/brainstorming/SKILL.md`.

### 4. Write the Project Brief
Using the SKILL.md template, write `docs/project-brief.md` directly using the Write
tool. Use the real app name, persona names, and capability descriptions from
UseCase.md — no generic placeholders.

Create the `docs/` directory if it doesn't exist.

### 5. Confirm
Show the project brief and ask:
> "Does this accurately capture what we're building? Any corrections before we move
> to architecture?"

Wait for confirmation, then say:
> "Project Brief saved to `docs/project-brief.md`. Run `/architecture` next to design
> the solution."
