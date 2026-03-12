# Phase 10: Coding Standards & Linting

## Purpose

Establish consistent coding standards, linting rules, and formatting conventions
that apply across all code app projects. This phase produces configuration files
and rules that can be copied into every new project as a baseline.

## Deliverable

A **Standards Starter Kit**: ESLint config, Prettier config, TypeScript config,
VS Code workspace settings, and a coding standards reference document.

## Why Standardise?

When delivering code apps across multiple client engagements, standardisation:
- Reduces onboarding time for new developers joining a project
- Makes code reviews faster and more objective
- Prevents style debates in PRs
- Ensures consistent quality regardless of which developer wrote the code
- Makes GitHub Copilot output more predictable (Copilot learns from surrounding code)

## TypeScript Configuration

### tsconfig.json (strict baseline)

```json
{
  "compilerOptions": {
    "target": "ES2020",
    "lib": ["ES2020", "DOM", "DOM.Iterable"],
    "module": "ESNext",
    "moduleResolution": "bundler",
    "jsx": "react-jsx",
    "strict": true,
    "noUncheckedIndexedAccess": true,
    "noImplicitReturns": true,
    "noFallthroughCasesInSwitch": true,
    "forceConsistentCasingInFileNames": true,
    "isolatedModules": true,
    "skipLibCheck": true,
    "esModuleInterop": true,
    "resolveJsonModule": true,
    "declaration": true,
    "declarationMap": true,
    "sourceMap": true,
    "baseUrl": ".",
    "paths": {
      "@/*": ["src/*"],
      "@components/*": ["src/components/*"],
      "@pages/*": ["src/pages/*"],
      "@hooks/*": ["src/hooks/*"],
      "@services/*": ["src/services/*"],
      "@types/*": ["src/types/*"],
      "@utils/*": ["src/utils/*"]
    },
    "outDir": "dist"
  },
  "include": ["src"],
  "exclude": ["node_modules", "dist", "src/generated"]
}
```

Key decisions:
- `strict: true` — non-negotiable. Catches entire categories of bugs
- `noUncheckedIndexedAccess` — forces handling undefined from array/object access
- `src/generated` excluded — auto-generated code shouldn't trigger lint errors
- Path aliases — cleaner imports, consistent across projects

## ESLint Configuration

### .eslintrc.cjs

```javascript
module.exports = {
  root: true,
  env: { browser: true, es2020: true, jest: true },
  extends: [
    'eslint:recommended',
    'plugin:@typescript-eslint/strict-type-checked',
    'plugin:@typescript-eslint/stylistic-type-checked',
    'plugin:react/recommended',
    'plugin:react/jsx-runtime',
    'plugin:react-hooks/recommended',
    'plugin:jsx-a11y/recommended',  // Accessibility linting
    'prettier',                      // Must be last — disables conflicting rules
  ],
  parser: '@typescript-eslint/parser',
  parserOptions: {
    ecmaVersion: 'latest',
    sourceType: 'module',
    project: ['./tsconfig.json'],
    tsconfigRootDir: __dirname,
  },
  plugins: ['@typescript-eslint', 'react', 'jsx-a11y', 'import'],
  settings: {
    react: { version: 'detect' },
  },
  ignorePatterns: ['dist/', 'node_modules/', 'src/generated/'],
  rules: {
    // TypeScript
    '@typescript-eslint/no-unused-vars': ['error', { argsIgnorePattern: '^_' }],
    '@typescript-eslint/explicit-function-return-type': ['warn', {
      allowExpressions: true,
      allowTypedFunctionExpressions: true,
    }],
    '@typescript-eslint/no-floating-promises': 'error',
    '@typescript-eslint/no-misused-promises': 'error',
    '@typescript-eslint/prefer-nullish-coalescing': 'warn',
    '@typescript-eslint/prefer-optional-chain': 'warn',
    
    // React
    'react/prop-types': 'off',  // Using TypeScript for prop validation
    'react/self-closing-comp': 'warn',
    'react/no-array-index-key': 'warn',
    'react-hooks/exhaustive-deps': 'warn',
    
    // Accessibility (critical for Phase 8 compliance)
    'jsx-a11y/anchor-is-valid': 'error',
    'jsx-a11y/click-events-have-key-events': 'error',
    'jsx-a11y/no-static-element-interactions': 'error',
    'jsx-a11y/label-has-associated-control': 'error',
    
    // Import ordering
    'import/order': ['warn', {
      groups: [
        'builtin', 'external', 'internal', 'parent', 'sibling', 'index'
      ],
      pathGroups: [
        { pattern: 'react', group: 'external', position: 'before' },
        { pattern: '@/**', group: 'internal' },
      ],
      'newlines-between': 'always',
      alphabetize: { order: 'asc' },
    }],
    
    // General quality
    'no-console': ['warn', { allow: ['warn', 'error'] }],
    'no-debugger': 'error',
    'prefer-const': 'error',
    'no-var': 'error',
    eqeqeq: ['error', 'always'],
  },
};
```

### Dependencies to install

```bash
npm install --save-dev \
  eslint \
  @typescript-eslint/parser \
  @typescript-eslint/eslint-plugin \
  eslint-plugin-react \
  eslint-plugin-react-hooks \
  eslint-plugin-jsx-a11y \
  eslint-plugin-import \
  eslint-config-prettier
```

## Prettier Configuration

### .prettierrc

```json
{
  "semi": true,
  "trailingComma": "all",
  "singleQuote": true,
  "printWidth": 90,
  "tabWidth": 2,
  "useTabs": false,
  "bracketSpacing": true,
  "arrowParens": "always",
  "endOfLine": "lf",
  "jsxSingleQuote": false
}
```

### .prettierignore

```
dist/
node_modules/
src/generated/
*.config.js
power.config.json
```

## VS Code Workspace Settings

### .vscode/settings.json

Ship this with every repo so any developer gets consistent behaviour on open:

```json
{
  "editor.defaultFormatter": "esbenp.prettier-vscode",
  "editor.formatOnSave": true,
  "editor.codeActionsOnSave": {
    "source.fixAll.eslint": "explicit",
    "source.organizeImports": "explicit"
  },
  "typescript.tsdk": "node_modules/typescript/lib",
  "typescript.enablePromptUseWorkspaceTsdk": true,
  "files.eol": "\n",
  "search.exclude": {
    "**/node_modules": true,
    "**/dist": true,
    "**/src/generated": true
  },
  "[typescript]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  },
  "[typescriptreact]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  }
}
```

### .vscode/extensions.json

Recommended extensions for the team:

```json
{
  "recommendations": [
    "esbenp.prettier-vscode",
    "dbaeumer.vscode-eslint",
    "github.copilot",
    "github.copilot-chat",
    "ms-vscode.vscode-typescript-next",
    "bradlc.vscode-tailwindcss",
    "usernamehw.errorlens",
    "axe-core.axe-accessibility-linter"
  ]
}
```

## Git Hooks (Pre-commit Quality Gate)

### Husky + lint-staged

```bash
npm install --save-dev husky lint-staged
npx husky init
```

### .husky/pre-commit
```bash
npx lint-staged
```

### package.json addition
```json
{
  "lint-staged": {
    "src/**/*.{ts,tsx}": [
      "eslint --fix",
      "prettier --write"
    ],
    "src/**/*.{json,md,css}": [
      "prettier --write"
    ]
  }
}
```

This ensures no code reaches the repo without passing lint and format checks.

## Code Review Checklist

Use this during PR reviews across all code app projects:

### Functionality
- [ ] Does the code do what the PR description says?
- [ ] Are edge cases handled (null, empty, error)?
- [ ] Are loading and error states implemented?

### TypeScript
- [ ] No `any` types (use `unknown` + type guards if needed)
- [ ] No type assertions (`as`) without comment explaining why
- [ ] Interfaces preferred over type aliases for object shapes
- [ ] Enums avoided in favour of const objects or union types

### React Patterns
- [ ] No business logic in components (extract to hooks or services)
- [ ] `useEffect` dependencies are correct
- [ ] No direct DOM manipulation
- [ ] Keys on list items are stable and unique (not array index)
- [ ] Memoisation (`useMemo`, `useCallback`) used only where measured benefit

### Accessibility
- [ ] All interactive elements keyboard-accessible
- [ ] Form inputs have associated labels
- [ ] No `jsx-a11y` lint warnings suppressed without comment
- [ ] ARIA attributes used correctly (prefer semantic HTML first)

### Performance
- [ ] No unnecessary re-renders (check with React DevTools)
- [ ] Large lists use virtualisation or pagination
- [ ] Images optimised and lazy-loaded where appropriate

### Security
- [ ] No secrets, tokens, or connection IDs in code
- [ ] User input sanitised before display (React handles this by default, but check `dangerouslySetInnerHTML`)
- [ ] No `eval()` or equivalent

## GitHub Copilot Prompt

```
@workspace Review the ESLint config in .eslintrc.cjs and suggest any
additional rules that would improve code quality for a Power Apps Code App
using React, TypeScript, and Fluent UI. Focus on rules that catch common
SPA bugs like unhandled promises, missing error boundaries, and
accessibility violations.
```

## Starter Kit Packaging

To reuse these standards across projects, maintain a template repo or npm
package containing:

```
code-app-standards/
├── .eslintrc.cjs
├── .prettierrc
├── .prettierignore
├── tsconfig.json
├── .vscode/
│   ├── settings.json
│   └── extensions.json
├── .husky/
│   └── pre-commit
├── .github/
│   └── pull_request_template.md
└── docs/
    └── coding-standards.md
```

Copy into new projects with:
```bash
npx degit [your-org]/code-app-standards ./standards-config
```
