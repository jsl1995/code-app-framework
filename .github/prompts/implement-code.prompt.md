---
agent: 'agent'
description: 'Phase 8: Build the app — scaffold, connect data sources, generate components, deploy'
tools: ['editFiles', 'createFile', 'readFile', 'runInTerminal']
---

You are guiding a developer through Phase 8 of building a Power Apps Code App.

Read the skill file for full guidance: [implement-code skill](../../agents/skills/implement-code/SKILL.md)
Refer to [pac-reference](../../agents/skills/pac-reference.md) for all CLI commands.

## Your task

Read `UseCase.md` and `docs/implementation-plan.md` first. Use the environment URL,
solution name, and connection references from those files — do not ask for values
already documented there.

Build the app step by step. **Do each step and confirm it works before moving on.**

### Step 1: Scaffold

Use the environment URL from UseCase.md and the app name from the Project Brief.

```bash
pac auth create --environment <environmentUrl from UseCase.md>
pac code init --name "<AppName from Project Brief>" --framework react-ts
npm install
```

### Step 2: Discover data sources

Use the connector manifest from `docs/implementation-plan.md` to run discovery commands
before adding each data source.

```bash
# For tabular sources — get exact dataset and table names
pac code list-datasets -a <apiName> -c <connectionId>
pac code list-tables -a <apiName> -c <connectionId> -d <datasetName>
```

### Step 3: Add data sources

Use connection references (not raw connection IDs) for all data sources.
Connection reference logical names are in `docs/implementation-plan.md`.

```bash
# Action-based connector
pac code add-data-source -a <apiName> -cr <connectionReferenceLogicalName> -s <solutionId>

# Tabular connector
pac code add-data-source -a <apiName> -cr <connectionReferenceLogicalName> -s <solutionId> -t <tableId> -d <datasetName>
```

After each command, verify generated files appear in `generated/services/`.
**Do not edit files in `generated/services/`** — they are owned by the CLI.

### Step 4: Set up coding standards

Install dev dependencies and copy config files from the plan:

```bash
npm install --save-dev eslint @typescript-eslint/parser @typescript-eslint/eslint-plugin \
  eslint-plugin-react eslint-plugin-react-hooks eslint-plugin-jsx-a11y eslint-config-prettier \
  prettier husky lint-staged jest @testing-library/react jest-axe
```

### Step 5: Convert mockups to production

For each mockup in `src/mockups/`, convert it to a production component:
- Replace sample data with calls to generated services
- Use the `{ data, isLoading, error, refetch }` hook pattern
- Wire filters to pass `$filter` and `$orderby` to service calls
- Keep all UI and responsive behaviour identical
- Save to `src/pages/`

### Step 6: Build and test locally

```bash
npm run dev
npm test
```

### Step 7: Deploy

Use the solution name from UseCase.md.

```bash
npm run build && pac code push --solution-unique-name <solutionName from UseCase.md>
```

Tell the developer: "App deployed. Verify it in the Power Apps portal at [environment URL from UseCase.md]. Share it with test users for UAT."
