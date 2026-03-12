---
agent: 'agent'
description: 'Phase 4: Generate working React/TypeScript UI mockups with real sample data'
tools: ['editFiles', 'createFile', 'readFile', 'runInTerminal']
---

You are guiding a developer through Phase 4 of building a Power Apps Code App.

Read the skill file for full guidance: [mock-up skill](../../agents/skills/mock-up/SKILL.md)

## Your task

Read `UseCase.md` and `docs/data-architecture.md`. Use the real entity names, persona
names, and core capabilities from UseCase.md when naming components and generating
sample data. Generate **real, working mockup components** — not wireframes.

1. **Generate** `src/test-utils/mockData.ts` with realistic sample data based on the
   actual entity schemas from `docs/data-architecture.md`
2. **Generate** each mockup as a complete `.tsx` file using Fluent UI React. Name
   components after the real entities and views in UseCase.md, for example:
   - `src/mockups/DashboardPage.tsx` — metric cards and recent items relevant to the use case
   - `src/mockups/[Entity]ListPage.tsx` — data table with filters, sort, pagination
   - `src/mockups/[Entity]DetailPage.tsx` — field grid, edit toggle, related items
   - `src/mockups/[Entity]CreateForm.tsx` — validated form with all field types
3. Each mockup must:
   - Render in the browser without API calls
   - Use realistic sample data that reflects the actual domain (use real-looking values, not Lorem Ipsum)
   - Include loading, error, and empty state variants
   - Have `// TODO: Replace with real service call` annotations

Tell the developer: "Mockups generated. Run `npm run dev` to preview. Type `/create-dataverse` to set up the real data layer."
