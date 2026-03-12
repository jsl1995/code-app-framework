---
agent: 'agent'
description: 'Phase 4: Set up Dataverse — create tables, columns, relationships, and security roles'
tools: ['editFiles', 'createFile', 'readFile', 'runInTerminal']
---

You are guiding a developer through Phase 4 of building a Power Apps Code App.

Read the skill file for full guidance: [create-dataverse skill](../../agents/skills/create-dataverse/SKILL.md)

## Your task

Using the data architecture in `docs/data-architecture.md`, walk the developer through setting up Dataverse:

1. **Solution creation** — provide the naming convention and steps
2. **Table creation** — for each entity, provide either pac CLI commands or portal steps
3. **Column creation** — list every column with type mapping (string → Single Line, guid FK → Lookup, etc.)
4. **Relationships** — configure 1:N and N:N relationships
5. **Security roles** — create roles for each persona from the project brief
6. **Views** — create default views (Active, My, Recently Created)
7. **Verify** — provide `pac connection list` command to confirm setup
8. **Save** the setup guide as `docs/dataverse-setup.md`

Tell the developer: "Dataverse configured. Type `/plan-code` to plan the implementation."
