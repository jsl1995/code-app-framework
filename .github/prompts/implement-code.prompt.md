---
agent: 'agent'
description: 'Phase 6: Build the app — scaffold, connect data sources, generate components, deploy'
tools: ['editFiles', 'createFile', 'readFile', 'runInTerminal']
---

You are guiding a developer through Phase 6 of building a Power Apps Code App.

Read the skill file for full guidance: [implement-code skill](../../agents/skills/implement-code/SKILL.md)

## Your task

Build the app step by step. **Do each step and confirm it works before moving on:**

### Step 1: Scaffold
```bash
pac auth create --environment <environmentUrl>
pac code init --name "<AppName>" --framework react-ts
npm install
```

### Step 2: Add data sources
Run each `pac code add-data-source` command from the connector manifest in `docs/implementation-plan.md`. After each one, verify the generated files appear in `generated/services/`.

### Step 3: Set up standards
Copy the config files generated in `/plan-code` into the project root. Install dev dependencies:
```bash
npm install --save-dev eslint @typescript-eslint/parser @typescript-eslint/eslint-plugin eslint-plugin-react eslint-plugin-react-hooks eslint-plugin-jsx-a11y eslint-config-prettier prettier husky lint-staged jest @testing-library/react jest-axe
```

### Step 4: Convert mockups to production
For each mockup in `src/mockups/`, convert it to a production component:
- Replace `mockData` imports with calls to the generated services
- Use the `useConnectorQuery` hook pattern
- Wire up filters to pass `$filter` and `$orderby` to service calls
- Keep all UI and responsive behaviour identical
- Save to `src/pages/`

### Step 5: Build and test locally
```bash
npm run start
npm test
```

### Step 6: Deploy
```bash
npm run build
pac code push --solution-unique-name <solutionName>
```

Tell the developer: "App deployed! Verify it in the Power Apps portal. Share it with test users for UAT."
