# npm-packages JS 

## How to create packages

### Recommended Structure
```
create-express-react-mongo/
│
├── bin/
│   └── index.js
│
├── templates/
│   └── default/
│       ├── client/
│       ├── server/
│       ├── package.json
│       └── ...
│
├── package.json
└── README.md
```

#### Step 1 — Initialize Package
mkdir create-express-react-mongo
cd create-express-react-mongo

npm init -y

#### Step 2 — Configure package.json

Update it like this:
```json
{
  "name": "create-express-react-mongo",
  "version": "1.0.0",
  "type": "module",
  "bin": {
    "create-express-react-mongo": "./bin/index.js"
  },
  "files": [
    "bin",
    "templates"
  ],
  "keywords": [
    "express",
    "react",
    "mongodb",
    "mern",
    "scaffold"
  ]
}
```

#### Step 3 — Install Helper Libraries

Useful packages:
```
npm install fs-extra chalk ora prompts execa
```

#### Step 4 — Create CLI Entry

Create:
```
bin/index.js
```

Add:

```javascript
#!/usr/bin/env node

import fs from "fs-extra";
import path from "path";
import chalk from "chalk";
import prompts from "prompts";
import { execa } from "execa";
import { fileURLToPath } from "url";

const __filename = fileURLToPath(import.meta.url);
const __dirname = path.dirname(__filename);

async function main() {
  console.log(
    chalk.green("\n Creating Express-React-Mongo app\n")
  );

  const response = await prompts({
    type: "text",
    name: "projectName",
    message: "Project name:",
    initial: "my-app"
  });

  const projectName = response.projectName;

  const targetDir = path.join(process.cwd(), projectName);

  // Copy template
  const templateDir = path.join(
    __dirname,
    "../templates/default"
  );

  await fs.copy(templateDir, targetDir);

  console.log(chalk.blue("\n Installing dependencies...\n"));

  // Install dependencies
  await execa("npm", ["install"], {
    cwd: targetDir,
    stdio: "inherit"
  });

  console.log(chalk.green("\n Project created successfully!\n"));

  console.log(`
Next steps:

  cd ${projectName}
  cd client -> npm install -> npm run dev
  cd server -> npm install -> npm run start
  `);
}

main().catch((err) => {
  console.error(err);
  process.exit(1);
});
```

#### Step 7 — Local Testing

Before publishing:

``` 
npm link
```

Then test:

```
create-express-react-mongo
```

OR:

```
npm create express-react-mongo
```

(using local linking tricks)

#### Step 8 — Publish to npm

Login:

```
npm login
```

Publish:

```
npm publish --access public
```

Now users can run:

```
npm create express-react-mongo
```
# npm-packages TS

### Setup should look like this
```
create-express-react-mongo/
├── bin/
│   └── index.ts
│
├── dist/
│   └── index.js
│
├── templates/
│   └── default/
│       ├── client/
│       ├── server/
│       ├── package.json
│       └── ...
├── package.json
├── tsconfig.json
```
#### 1. Install TypeScript

```
npm install -D typescript @types/node
```

#### 2. Create tsconfig.json

```
npx tsc --init
```

Update it:

```json
{
  "compilerOptions": {
    "target": "ES2020",
    "module": "NodeNext",
    "moduleResolution": "NodeNext",
    "outDir": "dist",
    "rootDir": "bin",
    "strict": true
  }
}
```

#### 3. Your bin/index.ts

Example:
```
#!/usr/bin/env node

console.log("CLI works!");
```
Even in TypeScript, keep:
```
#!/usr/bin/env node
```
at the top.

#### 4. Update package.json

IMPORTANT:

Your "bin" must point to the COMPILED JS file, not .ts.
```json
{
  "name": "create-express-react-mongo",
  "version": "1.0.0",
  "type": "module",

  "bin": {
    "create-express-react-mongo": "./dist/index.js"
  },

  "scripts": {
    "build": "tsc"
  }
}
```

#### 6. Test Locally

Run:
```
npm link
```
Then:
```
create-express-react-mongo
```
You should see:
```
CLI works!
```
### About packages:
| Package  | Purpose                 |
| -------- | ----------------------- |
| fs-extra | Copy files easily       |
| chalk    | Colored terminal output |
| ora      | Spinner                 |
| prompts  | CLI questions           |
| execa    | Run shell commands      |

## 1. create-react-express-mongo
- https://github.com/nimisha-527/create-react-express-mongo.git
