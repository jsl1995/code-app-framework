# Phase 4: Mock-ups

Read `agents/skills/mock-up/SKILL.md` for full guidance, then act directly.

## Steps

### 1. Read context
Read `UseCase.md`, `docs/project-brief.md`, and `docs/data-architecture.md`. Extract:
- Core capabilities (to determine which views are needed)
- Entity names and column definitions (for realistic sample data)
- Persona names and their primary tasks

Tell the developer:
> "[Phase 4/11: Mock-ups] I'll generate working React/TypeScript mockup components
> for: [list views derived from capabilities in UseCase.md]."

### 2. Identify required views
Based on the capabilities and entities, determine the standard view set:
- **Dashboard** — summary metrics and recent items
- **List** — paginated, filterable table of the primary entity
- **Detail** — read-only view of a single record with Edit/Delete actions
- **Create/Edit** — form for creating or updating a record
- Add additional views for secondary capabilities if needed

### 3. Generate mockup components
For each view, write a complete `.tsx` file to `src/mockups/` directly using the
Write tool. Each component must:
- Use **Fluent UI React v9** components (`@fluentui/react-components`)
- Use **realistic sample data** based on the real entity names and columns from
  `docs/data-architecture.md`
- Handle **three states**: loaded (sample data), loading (Spinner/Shimmer), error
- Include responsive layout using Fluent UI's layout primitives
- Annotate data-fetching spots with `// TODO: Replace with real service call`
- Be importable and renderable without a running backend

### 4. Write the mockup index
Write a `src/mockups/index.tsx` that renders all mockup components in a simple
tab or accordion layout so the developer can view them with `npm run dev`.

### 5. Confirm
Tell the developer:
> "Mockups written to `src/mockups/`. Run `npm run dev` to view them in the browser.
> Does this match your expectations for the UI? Any layout or flow changes before
> we plan the connectors?"

Wait for confirmation, then say:
> "Run `/connectors` next to map data sources to Power Platform connectors."
