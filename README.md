# React Starter Template

A modern, production-ready React template with Vite, TypeScript, Tailwind CSS, ShadCN UI, and more.

## Table of Contents

1. [Quick Start](#quick-start)
2. [Tech Stack Overview](#tech-stack-overview)
3. [Installation Guide](#installation-guide)
4. [Project Structure](#project-structure)
5. [Tools & Configuration](#tools--configuration)
6. [GitHub Actions CI/CD](#github-actions-cicd)
7. [Deployment Guides](#deployment-guides)

---

## Quick Start

```bash
# Clone the template
gh repo create my-react-app --template your-username/react-starter-template

# Navigate to project
cd my-react-app

# Install dependencies
npm install

# Start development server
npm run dev
```

---

## Tech Stack Overview

### Why Each Tool Matters

| Tool | Purpose | Why You Need It |
|------|---------|-----------------|
| **Vite** | Build tool & dev server | Lightning-fast HMR, optimized builds |
| **React TypeScript** | UI framework with type safety | Catch errors at compile time, better DX |
| **Tailwind CSS** | Utility-first CSS framework | Rapid UI development, consistent design system |
| **ShadCN UI** | Component library | Accessible, customizable components |
| **Prettier** | Code formatter | Consistent code style across team |
| **ESLint** | Linter | Catch bugs and enforce code quality |
| **Husky** | Git hooks | Enforce code quality before commits |
| **Storybook** | Component explorer | Build and test UI in isolation |
| **GitHub Actions** | CI/CD automation | Automated testing and deployment |

---

## Installation Guide

### Step 1: Create Vite + React TypeScript Project

```bash
npm create vite@latest my-app -- --template react-ts
cd my-app
```

**Why:** Vite provides instant server start and lightning-fast HMR (Hot Module Replacement). TypeScript adds static type checking for better developer experience and fewer runtime errors.

### Step 2: Install Tailwind CSS

```bash
npm install -D tailwindcss postcss autoprefixer
npx tailwindcss init -p
```

**Configure `tailwind.config.js`:**

```javascript
/** @type {import('tailwindcss').Config} */
export default {
  content: [
    "./index.html",
    "./src/**/*.{js,ts,jsx,tsx}",
  ],
  theme: {
    extend: {},
  },
  plugins: [],
}
```

**Add to `src/index.css`:**

```css
@tailwind base;
@tailwind components;
@tailwind utilities;
```

**Why:** Tailwind enables rapid UI development with utility classes. No context switching between CSS files and components.

### Step 3: Install ShadCN UI

```bash
npx shadcn@latest init
```

**Add components as needed:**

```bash
npx shadcn@latest add button
npx shadcn@latest add card
npx shadcn@latest add input
```

**Why:** ShadCN provides accessible, professional components that you own and customize. Built on Radix UI primitives with full accessibility support.

### Step 4: Install & Configure Prettier

```bash
npm install -D prettier
```

**Create `.prettierrc`:**

```json
{
  "semi": false,
  "singleQuote": true,
  "tabWidth": 2,
  "trailingComma": "es5",
  "printWidth": 100
}
```

**Create `.prettierignore`:**

```
node_modules
dist
build
.cache
```

**Why:** Prettier enforces consistent code formatting. No more debates about tabs vs spaces or semicolons.

### Step 5: Install & Configure ESLint

```bash
npm install -D eslint @typescript-eslint/parser @typescript-eslint/eslint-plugin eslint-plugin-react eslint-plugin-react-hooks
```

**Create `.eslintrc.cjs`:**

```javascript
module.exports = {
  root: true,
  env: { browser: true, es2020: true },
  extends: [
    'eslint:recommended',
    'plugin:@typescript-eslint/recommended',
    'plugin:react/recommended',
    'plugin:react/jsx-runtime',
    'plugin:react-hooks/recommended',
  ],
  ignorePatterns: ['dist', '.eslintrc.cjs'],
  parser: '@typescript-eslint/parser',
  plugins: ['react-refresh'],
  rules: {
    'react-refresh/only-export-components': [
      'warn',
      { allowConstantExport: true },
    ],
    '@typescript-eslint/no-unused-vars': ['warn', { argsIgnorePattern: '^_' }],
  },
}
```

**Why:** ESLint catches potential bugs and enforces coding best practices before they reach production.

### Step 6: Install & Configure Husky

```bash
# Install Husky
npm install -D husky

# Install lint-staged and commitlint
npm install -D lint-staged @commitlint/cli @commitlint/config-conventional

# Initialize Husky
npx husky init
```

**Update `package.json`:**

```json
{
  "scripts": {
    "prepare": "husky"
  },
  "lint-staged": {
    "*.{js,jsx,ts,tsx}": [
      "eslint --fix",
      "prettier --write"
    ],
    "*.{json,md,yml}": [
      "prettier --write"
    ]
  }
}
```

**Update `.husky/pre-commit`:**

```bash
npm run lint-staged
```

**Create `.husky/commit-msg`:**

```bash
npx --no -- commitlint --edit $1
```

**Create `commitlint.config.js`:**

```javascript
export default {
  extends: ['@commitlint/config-conventional'],
}
```

**Make hooks executable:**

```bash
chmod +x .husky/pre-commit .husky/commit-msg
```

**Why:** Husky runs checks before every commit. lint-staged only checks staged files (fast!), and commitlint enforces conventional commit messages like `feat: add new button`.

### Step 7: Install Storybook

```bash
npx storybook@latest init
```

**Run Storybook:**

```bash
npm run storybook
```

**Why:** Storybook lets you build UI components in isolation. Great for testing, documentation, and visual regression testing.

---

## Project Structure

```
src/
├── components/
│   ├── ui/              # ShadCN components
│   └── your-components/
├── stories/             # Storybook stories
├── lib/                 # Utilities
├── App.tsx
├── main.tsx
└── index.css
```

---

## Tools & Configuration

### Available Scripts

```bash
npm run dev          # Start dev server
npm run build        # Build for production
npm run lint         # Run ESLint
npm run format       # Run Prettier
npm run preview      # Preview production build
npm run storybook    # Start Storybook
npm run build-storybook  # Build Storybook
```

### Recommended VSCode Extensions

- ESLint
- Prettier
- Tailwind CSS IntelliSense
- Storybook

---

## GitHub Actions CI/CD

### Automatic Vercel Deployment

Create `.github/workflows/deploy.yml`:

```yaml
name: Deploy to Vercel

on:
  push:
    branches: [main]
  pull_request:
    branches: [main]

jobs:
  build-and-deploy:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: '20'
          cache: 'npm'

      - name: Install dependencies
        run: npm ci

      - name: Run linter
        run: npm run lint

      - name: Build project
        run: npm run build

      - name: Deploy to Vercel
        uses: amondnet/vercel-action@v25
        with:
          vercel-token: ${{ secrets.VERCEL_TOKEN }}
          vercel-org-id: ${{ secrets.VERCEL_ORG_ID }}
          vercel-project-id: ${{ secrets.VERCEL_PROJECT_ID }}
          vercel-args: '--prod'
```

**Why:** This workflow runs on every push to main and every PR. It:
1. Checks out code
2. Installs dependencies
3. Runs linter
4. Builds the project
5. Deploys to Vercel

### Getting Vercel Credentials

1. **VERCEL_TOKEN:** Create at https://vercel.com/account/tokens
2. **VERCEL_ORG_ID:** Run `npx vercel whoami --json`
3. **VERCEL_PROJECT_ID:** Get from Vercel project settings

### Add Secrets to GitHub

1. Go to your repo → Settings → Secrets and variables → Actions
2. Add the three secrets above

---

## Deployment Guides

### Deploy to Vercel (Recommended)

**Option 1: CLI**

```bash
npm i -g vercel
vercel
```

**Option 2: Git Integration**

1. Push code to GitHub
2. Import project in Vercel
3. Configure build settings:
   - Build Command: `npm run build`
   - Output Directory: `dist`
4. Deploy!

**Why Vercel:** Zero-config deployments, automatic SSL, global CDN, and instant rollbacks.

### Deploy to GitHub Pages

This template includes GitHub Pages deployment support.

#### Step 1: Update `package.json`

Add homepage field:

```json
{
  "homepage": "https://yourusername.github.io/repo-name"
}
```

#### Step 2: Update Vite Config

In `vite.config.ts`:

```typescript
import { defineConfig } from 'vite'
import react from '@vitejs/plugin-react'

export default defineConfig({
  plugins: [react()],
  base: '/repo-name/',  // Your repository name
})
```

#### Step 3: Create Deployment Workflow

Create `.github/workflows/pages.yml`:

```yaml
name: Deploy to GitHub Pages

on:
  push:
    branches: [main]
  workflow_dispatch:

permissions:
  contents: read
  pages: write
  id-token: write

concurrency:
  group: pages
  cancel-in-progress: false

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: Setup Node
        uses: actions/setup-node@v4
        with:
          node-version: '20'
          cache: 'npm'

      - name: Install dependencies
        run: npm ci

      - name: Lint
        run: npm run lint

      - name: Build
        run: npm run build

      - name: Upload artifact
        uses: actions/upload-pages-artifact@v3
        with:
          path: ./dist

  deploy:
    needs: build
    runs-on: ubuntu-latest
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    steps:
      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v4
```

#### Step 4: Configure GitHub Pages

1. Go to Repository → Settings → Pages
2. Source: Select "GitHub Actions"
3. Save

#### Step 5: Push and Deploy

```bash
git add .
git commit -m "Initial commit"
git push origin main
```

Check the Actions tab to see deployment progress. Your site will be available at `https://yourusername.github.io/repo-name/`

**Why GitHub Pages:** Free hosting for static sites, tied directly to your repo, no additional accounts needed.

---

## Tips & Best Practices

### Before Committing

- Always run `npm run lint` to check for errors
- Use `npm run format` before committing
- Husky will run these automatically

### Adding New Components

```bash
# Add ShadCN component
npx shadcn@latest add button

# Create Storybook story
# Edit src/stories/Button.stories.tsx
```

### Adding New Dependencies

```bash
# Production dependency
npm install package-name

# Development dependency
npm install -D package-name
```

### Updating Dependencies

```bash
# Check outdated packages
npm outdated

# Update all packages
npm update

# Or update specific package
npm install package@latest
```

---

## Troubleshooting

### Husky Not Running

```bash
# Reinstall husky
npm uninstall husky
npm install -D husky
npx husky init
```

### ESLint Errors in VSCode

1. Install ESLint extension
2. Add to `.vscode/settings.json`:

```json
{
  "editor.codeActionsOnSave": {
    "source.fixAll.eslint": true
  },
  "eslint.validate": ["typescript", "typescriptreact"]
}
```

### Build Errors

1. Clear node_modules and reinstall:
   ```bash
   rm -rf node_modules
   npm install
   ```

2. Clear Vite cache:
   ```bash
   rm -rf node_modules/.vite
   ```

---

## License

MIT
