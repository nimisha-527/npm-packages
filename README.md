# npm-packages

Complete guide to creating, testing, and publishing Node.js CLI packages to npm. Includes templates for both JavaScript and TypeScript projects.

## Table of Contents

- [Prerequisites](#prerequisites)
- [Quick Start](#quick-start)
- [JavaScript Packages](#javascript-packages)
- [TypeScript Packages](#typescript-packages)
- [Local Testing with Verdaccio](#local-testing-with-verdaccio)
- [Publishing to npm](#publishing-to-npm)
- [Troubleshooting](#troubleshooting)
- [Published Examples](#published-examples)

## Prerequisites

- Node.js v16+ and npm v7+
- npm account for publishing (create at [npmjs.com](https://www.npmjs.com))
- Understanding of npm `bin` field and package structure

## Quick Start

```bash
# Create a new package
mkdir my-cli-tool
cd my-cli-tool
npm init -y

# Install helper libraries
npm install fs-extra chalk ora prompts execa

# Create bin directory and entry point
mkdir -p bin
echo '#!/usr/bin/env node
console.log("🚀 CLI works!");' > bin/index.js
chmod +x bin/index.js

# Update package.json bin field (see examples below)
```

---

# JavaScript Packages

## Recommended Structure

```
<package-name>/
│
├── bin/
│   └── index.js
│
├── templates/
│   ├── <your-template-name>/
│   ├── <another-template>/
│   └── ...
│
├── src/          (optional)
│   └── utils.js
│
├── package.json
├── .npmrc
└── README.md
```

## Step-by-Step Guide

### Step 1 — Initialize Package

```bash
mkdir <package-name>
cd <package-name>
npm init -y
```

### Step 2 — Configure package.json

Update `package.json` with:

```json
{
  "name": "@yourusername/package-name",
  "version": "1.0.0",
  "description": "Your package description",
  "type": "module",
  "bin": {
    "command-name": "./bin/index.js"
  },
  "files": [
    "bin",
    "templates",
    "src"
  ],
  "scripts": {
    "prepublishOnly": "node bin/index.js"
  },
  "keywords": ["cli", "generator", "boilerplate"],
  "author": "Your Name",
  "license": "MIT"
}
```

**Key points:**
- `"bin"` points to your CLI entry file
- `"files"` array prevents unnecessary files from being published
- Use scoped name (`@username/package`) to avoid conflicts

### Step 3 — Install Helper Libraries

```bash
npm install fs-extra chalk ora prompts execa
```

### Step 4 — Create CLI Entry Point

Create `bin/index.js`:

```javascript
#!/usr/bin/env node

import chalk from 'chalk';
import prompts from 'prompts';
import ora from 'ora';

console.log(chalk.blue.bold('Welcome to my CLI!'));

const response = await prompts({
  type: 'text',
  name: 'projectName',
  message: 'What is your project name?',
  initial: 'my-project'
});

const spinner = ora('Creating project...').start();

// Your CLI logic here

spinner.succeed(chalk.green('Project created!'));
```

Remember to keep this at the top:
```javascript
#!/usr/bin/env node
```

**Refer to published packages** for real-world examples:
- [create-react-express-mongo](https://github.com/nimisha-527/create-react-express-mongo) — Direct installation with limited tech stack
- [create-webapp-kit](https://github.com/nimisha-527/create-webapp-kit) — Multiple tech stack choices

---

# TypeScript Packages

## Recommended Structure

```
<package-name>/
├── bin/
│   └── index.ts
│
├── dist/
│   └── index.js
│
├── src/          (optional)
│   ├── utils.ts
│   └── helpers.ts
│
├── templates/
│   └── default/
│       ├── <your-folder>/
│       ├── <another-folder>/
│       └── ...
│
├── package.json
├── tsconfig.json
├── .npmrc
└── README.md
```

## Setup Guide

### Step 1 — Install TypeScript

```bash
npm install -D typescript @types/node
```

### Step 2 — Create tsconfig.json

```bash
npx tsc --init
```

Update it to:

```json
{
  "compilerOptions": {
    "target": "ES2020",
    "module": "NodeNext",
    "moduleResolution": "NodeNext",
    "outDir": "./dist",
    "rootDir": "./bin",
    "strict": true,
    "esModuleInterop": true,
    "skipLibCheck": true,
    "forceConsistentCasingInFileNames": true
  },
  "include": ["bin/**/*"],
  "exclude": ["node_modules", "dist"]
}
```

### Step 3 — Create bin/index.ts

```typescript
#!/usr/bin/env node

import chalk from 'chalk';
import prompts from 'prompts';
import ora from 'ora';

console.log(chalk.blue.bold('Welcome to my CLI!'));

const response = await prompts({
  type: 'text',
  name: 'projectName',
  message: 'What is your project name?',
  initial: 'my-project'
});

const spinner = ora('Creating project...').start();

// Your CLI logic here

spinner.succeed(chalk.green('Project created!'));
```

**Important:** Keep this at the very top:
```typescript
#!/usr/bin/env node
```

### Step 4 — Update package.json

```json
{
  "name": "@yourusername/package-name",
  "version": "1.0.0",
  "description": "Your package description",
  "type": "module",
  "bin": {
    "command-name": "./dist/index.js"
  },
  "files": [
    "dist",
    "templates"
  ],
  "scripts": {
    "build": "tsc",
    "prepublishOnly": "npm run build"
  },
  "devDependencies": {
    "typescript": "^5.0.0",
    "@types/node": "^20.0.0"
  },
  "dependencies": {
    "chalk": "^5.0.0",
    "fs-extra": "^11.0.0",
    "ora": "^7.0.0",
    "prompts": "^2.4.0",
    "execa": "^6.0.0"
  }
}
```

**Critical:** The `"bin"` field **must** point to the **compiled JS file** (`./dist/index.js`), not the TypeScript source.

---

## Helper Libraries Reference

| Package  | Purpose                      | Usage                                                      |
|----------|------------------------------|------------------------------------------------------------|
| fs-extra | File system utilities        | Copy files, remove directories with ease                  |
| chalk    | Colored terminal output      | `chalk.blue('text')`, `chalk.red.bold('error')`           |
| ora      | Loading spinners             | `ora('Loading...').start()`, `.succeed()`, `.fail()`      |
| prompts  | Interactive CLI questions    | Text input, choices, confirmation dialogs                 |
| execa    | Execute shell commands       | Run scripts, npm commands, git operations                 |

---

## Local Testing with Verdaccio

Test your package locally before publishing to npm using Verdaccio, a lightweight private npm registry.

### Step 1 — Verify Package Contents

Check which folders will be included in the npm package:

```bash
npm pack
# or
npm link
```

Review the generated `.tgz` file or symlink to ensure all necessary folders are included.

### Step 2 — Install Verdaccio

Choose your package manager:

**npm:**
```bash
npm install -g verdaccio
```

**yarn:**
```bash
yarn global add verdaccio
```

**pnpm:**
```bash
pnpm install -g verdaccio
```

### Step 3 — Start Verdaccio

```bash
verdaccio
```

Keep this terminal running. Verdaccio will start on `http://localhost:4873`.

### Step 4 — Configure npm Registry

In a new terminal, configure npm to use the local registry:

```bash
npm set registry http://localhost:4873/
```

Or create/update `.npmrc` in your project:

```
registry=http://localhost:4873/
```

Add `.npmrc` to `.gitignore` if this is a public repository.

### Step 5 — Create Verdaccio User

```bash
npm adduser --registry http://localhost:4873/
# or login as existing user:
npm login --registry http://localhost:4873/
```

### Step 6 — Use Temporary Version

It's best practice to use a temporary version when testing:

```bash
# In package.json, change version to:
"version": "1.0.0-test-1"
```

This keeps your main version intact and avoids confusion.

### Step 7 — Build and Publish

```bash
# Build (TypeScript only)
npm run build

# Publish to Verdaccio
npm publish --registry http://localhost:4873/
# or simply:
npm publish
```

### Step 8 — Test Installation Locally

Create a new test folder:

```bash
mkdir test-install
cd test-install

# Use your package
npm create <package-name>@1.0.0-test-1

# or install it
npm install <package-name>@1.0.0-test-1
```

Your CLI will run exactly as it would when installed from npm.

### Step 9 — Reset Registry (Important!)

Before publishing to npm, reset your registry:

```bash
npm set registry https://registry.npmjs.org/
```

---

## Publishing to npm

### Pre-Publish Checklist

- ✅ Verify registry is set to npm:
  ```bash
  npm config get registry
  # Should output: https://registry.npmjs.org/
  ```

- ✅ Update `.npmrc` (if exists):
  ```
  registry=https://registry.npmjs.org/
  ```

- ✅ Increment version in `package.json`
- ✅ Update `CHANGELOG.md` or similar
- ✅ Test locally with Verdaccio first

### Build (TypeScript only)

```bash
npm run build
```

### Login to npm

```bash
npm login
# Enter your npm username, password, and email
```

### Publish

```bash
npm publish --access public
```

For scoped packages (`@username/package`), you may need:

```bash
npm publish --access public --@yourusername:registry=https://registry.npmjs.org/
```

### Success!

Users can now install your package:

```bash
npm create <package-name>@latest
```

---

## Troubleshooting

### Issue: "Permission denied" when running CLI

**Solution:** Make sure the bin file has executable permissions:
```bash
chmod +x bin/index.js   # JS
chmod +x bin/index.ts   # TS (before compilation)
```

### Issue: CLI not found after `npm install -g`

**Solution:** Check that `package.json` has a valid `bin` field:
```bash
npm list -g <package-name>
# Verify it's actually installed
```

### Issue: TypeScript compilation errors

**Solution:** Ensure `tsconfig.json` has correct paths:
```json
{
  "compilerOptions": {
    "outDir": "./dist",
    "rootDir": "./bin"
  }
}
```

Then rebuild:
```bash
npm run build
```

### Issue: Registry conflicts (local vs npm)

**Solution:** Check and reset your registry:
```bash
npm config get registry
npm config set registry https://registry.npmjs.org/
```

### Issue: "npm ERR! 403 Forbidden" when publishing

**Solution:** 
- Verify you're logged in: `npm whoami`
- Check package name doesn't conflict with existing package on npm
- For scoped packages, ensure you have publish permissions

### Issue: Package works locally but fails after npm install

**Solution:** Verify `files` array in `package.json`:
```bash
npm pack   # Create .tgz
tar tzf <package-name>.tgz   # List contents
```

Make sure all necessary files are included.

### Issue: Shebang line causes issues on Windows

**Solution:** Use `.cmd` wrapper. Add to `package.json`:
```json
{
  "bin": {
    "package-name": "./dist/index.js"
  }
}
```

npm automatically creates `.cmd` wrappers on Windows.

---

## Published Examples

Learn from these published packages:

### 1. [create-react-express-mongo](https://www.npmjs.com/package/create-react-express-mongo)
- **GitHub:** https://github.com/nimisha-527/create-react-express-mongo
- **Use case:** Direct installation with a fixed tech stack (React + Express + MongoDB)
- **Learn:** Basic project scaffolding, file copying with fs-extra

### 2. [create-webapp-kit](https://www.npmjs.com/package/create-webapp-kit)
- **GitHub:** https://github.com/nimisha-527/create-webapp-kit
- **Use case:** Interactive tech stack selection during project creation
- **Learn:** Dynamic prompts, conditional template generation, advanced CLI patterns

---

## Additional Resources

- [npm Package Documentation](https://docs.npmjs.com/creating-and-publishing-unscoped-public-packages)
- [TypeScript Handbook](https://www.typescriptlang.org/docs/)
- [Verdaccio Documentation](https://verdaccio.org/)
- [Node.js Executable Files](https://nodejs.org/en/knowledge/command-line/how-to-write-command-line-applications-in-nodejs/)

---

## License

MIT
