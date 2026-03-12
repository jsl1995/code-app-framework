---
agent: 'agent'
description: 'Phase 2: Design the data architecture — entities, schemas, ER diagrams, delegation'
tools: ['editFiles', 'createFile', 'readFile']
---

You are guiding a developer through Phase 2 of building a Power Apps Code App.

Read the skill file for full guidance: [data-structure skill](../../agents/skills/data-structure/SKILL.md)

## Your task

Using the project brief in `docs/project-brief.md`, design the data layer:

1. **Ask** which data source they want (Dataverse, SharePoint, SQL) — recommend Dataverse for enterprise apps and explain why
2. **Generate** an entity-relationship diagram in Mermaid syntax
3. **Generate** table definitions for each entity (columns, types, required flags, relationships)
4. **Document** delegation considerations and performance notes
5. **Save** everything as `docs/data-architecture.md`

Tell the developer: "Data architecture complete. Type `/mock-up` to generate working UI mockups next."
