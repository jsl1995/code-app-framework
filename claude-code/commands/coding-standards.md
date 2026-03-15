# Phase 10: Coding Standards

Read `agents/skills/coding-standards/SKILL.md` for full guidance, then act directly.

Run this at the start of Phase 6 before any application code is written.

## Steps

### 1. Install dependencies
Run using the Bash tool:
```bash
npm install --save-dev \
  @typescript-eslint/parser @typescript-eslint/eslint-plugin \
  eslint eslint-config-prettier eslint-plugin-jsx-a11y eslint-plugin-react-hooks \
  prettier \
  husky lint-staged
npx husky init
```

### 2. Write configuration files
Write each file directly using the Write tool. Use the exact configurations from
`agents/skills/coding-standards/SKILL.md`:

- `tsconfig.json` — strict mode, `noUncheckedIndexedAccess`, path aliases,
  exclude `src/generated`
- `.eslintrc.cjs` — `@typescript-eslint/strict-type-checked`, `jsx-a11y`,
  `react-hooks`, ignore `src/generated`
- `.prettierrc` — singleQuote, trailingComma all, printWidth 90
- `jest.config.ts` — `setupFilesAfterEnv`, exclude `src/generated` from coverage,
  path alias mapping
- `.husky/pre-commit` — `npx lint-staged`
- `.vscode/settings.json` — format-on-save, ESLint auto-fix
- `.vscode/extensions.json` — Copilot, ESLint, Prettier recommendations

### 3. Update package.json
Add the `lint-staged` configuration to `package.json`:
```json
"lint-staged": {
  "src/**/*.{ts,tsx}": ["eslint --fix", "prettier --write"],
  "src/**/*.{css,json,md}": ["prettier --write"]
}
```

### 4. Write the PR review checklist
Write `.github/pull_request_template.md` using the PR checklist template from
the SKILL.md (code quality, accessibility, security, performance sections).

### 5. Verify
Run:
```bash
npm run lint
```

Fix any issues. When clean, tell the developer:
> "Coding standards configured. ESLint, Prettier, and Husky pre-commit hooks are
> active. All future commits will be linted automatically."
