# Phase 6: Implement Code

Read `agents/skills/implement-code/SKILL.md` and `agents/skills/pac-reference.md`
for full guidance, then act directly.

## Steps

### 1. Read context
Read `UseCase.md`, `docs/connector-manifest.md`, `docs/data-architecture.md`, and
any mockup files in `src/mockups/`. Extract the solution name, environment URL,
and connector manifest.

Tell the developer:
> "[Phase 6/11: Implement Code] Let's build it. I'll scaffold the project, wire up
> data sources, convert mockups to production components, and deploy."

### 2. Set up coding standards
Before writing any app code, run `/coding-standards` (or apply Phase 10 inline):
- Generate `tsconfig.json`, `.eslintrc.cjs`, `.prettierrc`, `jest.config.ts`
- Install and configure Husky + lint-staged
- Write `.vscode/settings.json` and `.vscode/extensions.json`

### 3. Scaffold the project
Run these commands using the Bash tool:

```bash
pac auth create --environment <environmentUrl>
pac code init --name "<AppName>" --framework react-ts
cd <AppName>
npm install
```

Verify the scaffold succeeded by checking that `power.config.json` and
`src/App.tsx` were created.

### 4. Discover and add data sources
For each connector in `docs/connector-manifest.md`, run the discovery commands
and then `pac code add-data-source`. Use the exact dataset and table names —
they are case-sensitive. Use connection references for ALM portability.

After each `add-data-source` command, verify the generated files exist in
`generated/services/`.

### 5. Set up error handling
Apply Phase 11 inline before converting mockups:
- Write `src/components/ErrorBoundary.tsx`
- Write `src/utils/withRetry.ts`
- Write error state UI components

### 6. Convert mockups to production components
For each file in `src/mockups/`:
1. Read the mockup file using the Read tool
2. Replace `// TODO: Replace with real service call` annotations with actual
   calls to the generated service
3. Replace hardcoded sample data with the hook pattern (`useEntityName`)
4. Write the production component to `src/components/` or `src/pages/`
5. Write the corresponding hook to `src/hooks/`

Follow the hook pattern from `agents/skills/connectors/SKILL.md`:
`{ data, isLoading, error, refetch }`

### 7. Set up routing
Write `src/App.tsx` with React Router routes matching the views from Phase 4.
Include a global `ErrorBoundary` wrapper and an SDK initialisation loading state.

### 8. Local development verification
Run:
```bash
npm run dev
```
Ask the developer to verify: "Does the app load and all connector calls succeed?"
Fix any issues before proceeding.

### 9. Build and deploy
```bash
npm run build
pac code push --solution-unique-name <solutionName>
```

Confirm deployment by asking the developer to open the app in the Power Apps
portal and verify all connectors function with a test user.

### 10. Post-deployment checklist
Run through the checklist in the SKILL.md:
- [ ] App appears in Power Apps portal
- [ ] Consent dialog appears correctly
- [ ] All connectors function with a test user
- [ ] DLP policies don't block the app
- [ ] Error states display correctly
- [ ] App is shared with the appropriate security group

When confirmed, say:
> "App deployed. Run `/testing` to set up the test suite next."
