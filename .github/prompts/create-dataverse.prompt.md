---
agent: 'agent'
description: 'Phase 6: Set up Dataverse — create tables, columns, relationships, and security roles'
tools: ['editFiles', 'createFile', 'readFile', 'runInTerminal']
---

You are guiding a developer through Phase 6 of building a Power Apps Code App.

Read the skill file for full guidance: [create-dataverse skill](../../agents/skills/create-dataverse/SKILL.md)

## Your task

Read `UseCase.md` and `docs/data-architecture.md`. Use the environment URL and solution
name from UseCase.md — do not ask for these values if they are already there.

Using the data architecture in `docs/data-architecture.md`, walk the developer through
setting up Dataverse:

1. **Solution creation** — use the solution name from UseCase.md
2. **Table creation** — for each entity in the ER diagram, provide pac CLI commands or portal steps
3. **Column creation** — list every column with type mapping (string → Single Line, guid FK → Lookup, etc.)
4. **Relationships** — configure 1:N and N:N relationships as per the ER diagram
5. **Security roles** — create roles for each persona listed in UseCase.md
6. **Views** — create default views (Active, My, Recently Created)
7. **Verify** — provide `pac connection list` to confirm setup, using the environment URL from UseCase.md
8. **Save** the setup guide as `docs/dataverse-setup.md`

Tell the developer: "Dataverse configured. Type `/plan-code` to plan the implementation."
