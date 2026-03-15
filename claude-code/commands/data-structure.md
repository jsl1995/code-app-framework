# Phase 3: Data Structure

Read `agents/skills/data-structure/SKILL.md` for full guidance, then act directly.

## Steps

### 1. Read context
Read `UseCase.md` and `docs/project-brief.md`. Extract the preferred data source,
row counts, existing tables, and any Dataverse environment details.

Tell the developer:
> "[Phase 3/11: Data Structure] Based on your use case I'll select the right data
> source and design the entity schema. [Summary of data source preference from
> UseCase.md]."

### 2. Select the data source
Apply the decision framework from the SKILL.md:
- Row counts, relational complexity, existing Dataverse usage, DLP constraints
- Recommend the appropriate source and explain why

### 3. Design the schema
Using the real entity names from UseCase.md, define:
- All tables with column names, data types, and constraints
- Relationships (1:N, N:N) with foreign key references
- A **Mermaid ER diagram** using real entity names

### 4. Address delegation and performance
Document:
- Which columns are delegation-safe with each connector
- Recommended server-side `$filter` patterns
- Caching strategy for lookup/reference data

### 5. Write the data architecture document
Write `docs/data-architecture.md` directly using the Write tool. Include the ER
diagram, table definitions, and delegation notes.

### 6. Confirm
Show the data architecture and ask:
> "Does this data model match your requirements? Any schema changes before we design
> the mockups?"

Wait for confirmation, then say:
> "Data architecture saved to `docs/data-architecture.md`. Run `/mock-up` next."
