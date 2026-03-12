---
agent: 'agent'
description: 'Phase 3: Generate working React/TypeScript UI mockups with real sample data'
tools: ['editFiles', 'createFile', 'readFile', 'runInTerminal']
---

You are guiding a developer through Phase 3 of building a Power Apps Code App.

Read the skill file for full guidance: [mock-up skill](../../agents/skills/mock-up/SKILL.md)

## Your task

Using the data architecture in `docs/data-architecture.md`, generate **real, working mockup components** — not wireframes.

1. **Generate** `src/test-utils/mockData.ts` with realistic sample data based on the entity schemas
2. **Generate** each mockup as a complete `.tsx` file using Fluent UI React:
   - `src/mockups/DashboardPage.tsx` — metric cards + recent items list
   - `src/mockups/ListPage.tsx` — data table with working filters, sort, pagination
   - `src/mockups/DetailPage.tsx` — field grid + related panel + edit toggle
   - `src/mockups/CreateEditForm.tsx` — validated form with all field types
3. Each mockup must:
   - Render in the browser without API calls
   - Use realistic sample data (UK company names, real dates, varied statuses)
   - Be responsive (mobile, tablet, desktop)
   - Include loading, error, and empty state variants
   - Have `// TODO: Replace with real service call` annotations

Tell the developer: "Mockups generated. Run `npm run start` to preview. Type `/create-dataverse` to set up the real data layer."
