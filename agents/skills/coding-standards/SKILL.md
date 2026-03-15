# Phase 10: Coding Standards

## Purpose

Establish consistent, enforceable coding standards at the start of Phase 6 so that all
generated and hand-written code meets the same quality bar. Set these up once — CI and
editor tooling enforce them automatically.

## Deliverable

A configured project with ESLint, Prettier, TypeScript strict mode, Husky pre-commit
hooks, and VS Code settings committed to the repository.

## TypeScript Configuration

```json
// tsconfig.json (key settings)
{
  "compilerOptions": {
    "strict": true,
    "noUncheckedIndexedAccess": true,
    "exactOptionalPropertyTypes": true,
    "noImplicitReturns": true,
    "baseUrl": "src",
    "paths": {
      "@components/*": ["components/*"],
      "@hooks/*": ["hooks/*"],
      "@services/*": ["services/*"],
      "@types/*": ["types/*"],
      "@utils/*": ["utils/*"]
    }
  },
  "exclude": ["src/generated"]
}
```

> `src/generated/` must be excluded from strict checks — it is auto-generated code.

## ESLint Configuration

```js
// .eslintrc.cjs
module.exports = {
  root: true,
  parser: '@typescript-eslint/parser',
  parserOptions: { project: './tsconfig.json', tsconfigRootDir: __dirname },
  plugins: ['@typescript-eslint', 'jsx-a11y', 'react-hooks'],
  extends: [
    'eslint:recommended',
    'plugin:@typescript-eslint/strict-type-checked',
    'plugin:jsx-a11y/recommended',
    'plugin:react-hooks/recommended',
    'prettier',
  ],
  rules: {
    '@typescript-eslint/no-floating-promises': 'error',
    '@typescript-eslint/no-explicit-any': 'error',
  },
  ignorePatterns: ['src/generated/**/*'],
};
```

## Prettier Configuration

```json
// .prettierrc
{
  "singleQuote": true,
  "trailingComma": "all",
  "printWidth": 90,
  "semi": true,
  "tabWidth": 2
}
```

## Husky + lint-staged

```bash
# Install
npm install --save-dev husky lint-staged
npx husky init
```

```json
// package.json
{
  "lint-staged": {
    "src/**/*.{ts,tsx}": ["eslint --fix", "prettier --write"],
    "src/**/*.{css,json,md}": ["prettier --write"]
  }
}
```

```bash
# .husky/pre-commit
npx lint-staged
```

## Jest Configuration

```typescript
// jest.config.ts
export default {
  preset: 'ts-jest',
  testEnvironment: 'jsdom',
  setupFilesAfterEnv: ['<rootDir>/src/test-utils/setup.ts'],
  coveragePathIgnorePatterns: [
    '/node_modules/',
    '/src/generated/',   // exclude auto-generated code from coverage
  ],
  moduleNameMapper: {
    '^@components/(.*)$': '<rootDir>/src/components/$1',
    '^@hooks/(.*)$':      '<rootDir>/src/hooks/$1',
    '^@services/(.*)$':   '<rootDir>/src/services/$1',
    '^@types/(.*)$':      '<rootDir>/src/types/$1',
    '^@utils/(.*)$':      '<rootDir>/src/utils/$1',
  },
};
```

## VS Code Settings

```json
// .vscode/settings.json
{
  "editor.formatOnSave": true,
  "editor.defaultFormatter": "esbenp.prettier-vscode",
  "editor.codeActionsOnSave": {
    "source.fixAll.eslint": "explicit"
  },
  "typescript.tsdk": "node_modules/typescript/lib"
}
```

```json
// .vscode/extensions.json
{
  "recommendations": [
    "GitHub.copilot",
    "GitHub.copilot-chat",
    "dbaeumer.vscode-eslint",
    "esbenp.prettier-vscode",
    "ms-vscode.vscode-typescript-next"
  ]
}
```

## Naming Conventions

| Item | Convention | Example |
|------|-----------|---------|
| Components | PascalCase | `AccountList.tsx` |
| Hooks | camelCase with `use` | `useAccounts.ts` |
| Utils | camelCase | `formatCurrency.ts` |
| Types | PascalCase | `Account.ts` |
| Tests | `[source].test.tsx` | `AccountList.test.tsx` |
| Constants | UPPER_SNAKE_CASE | `MAX_PAGE_SIZE` |
| Branches | `feature/[ticket]-description` | `feature/42-add-filters` |
| Commits | Conventional Commits | `feat:`, `fix:`, `docs:`, `chore:` |

## PR Review Checklist

```markdown
## PR Review Checklist

### Code quality
- [ ] TypeScript strict — no `any`, no type assertions without comment
- [ ] No unhandled promise rejections (`await` or `.catch()` everywhere)
- [ ] No edits to `src/generated/` files
- [ ] Tests added or updated for changed behaviour

### Accessibility
- [ ] No new axe violations (`jest-axe` passes)
- [ ] New interactive elements are keyboard-accessible

### Security
- [ ] No secrets or credentials in code
- [ ] No `dangerouslySetInnerHTML` without sanitisation

### Performance
- [ ] No unnecessary re-renders (check with React DevTools)
- [ ] Large lists use pagination or virtualisation
```

## GitHub Copilot Prompt

```
@workspace Generate all project configuration files for a React TypeScript code app:
1. tsconfig.json with strict mode, noUncheckedIndexedAccess, and path aliases
   (exclude src/generated from strict checks)
2. .eslintrc.cjs with typescript-strict, jsx-a11y, and react-hooks plugins
   (ignore src/generated)
3. .prettierrc with singleQuote, trailingComma all, printWidth 90
4. jest.config.ts excluding src/generated from coverage, with path alias mapping
5. .vscode/settings.json with format-on-save and ESLint auto-fix
6. .vscode/extensions.json recommending Copilot, ESLint, Prettier
Place each file at the project root or .vscode/ as appropriate.
```
