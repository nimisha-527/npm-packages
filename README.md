# npm-packages JS 

## How to create packages

### Recommended Structure
```
<package-name>/
│
├── bin/
│   └── index.js
│
├── templates/
│       ├── <your file/folder>
│       ├── <your file/folder>
│       └── ...
│
├── package.json
└── README.md
```

#### Step 1 — Initialize Package
mkdir <package-name>
cd <package-name>

npm init -y

#### Step 2 — Configure package.json

##### Refer: 
Section "Custom packages Created and Published" for already published packages

#### Step 3 — Install Helper Libraries

Useful packages:
```
npm install fs-extra chalk ora prompts execa
```

#### Step 4 — Create CLI Entry

##### Create: bin/index.js

Keep
```
#!/usr/bin/env node
```
at the top.

##### Refer: Section "Custom packages Created and Published" for already published packages

#### Step 5. Refer Section *Local Testing* and *Publish to npm* after all the code has been updated according to your file structure.

# npm-packages TS

### Setup should look like this
```
<package-name>/
├── bin/
│   └── index.ts
│
├── dist/
│   └── index.js
│
├── templates/
│   └── default/
│       ├── <your folder>/
│       ├── <your folder>/
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

Keep
```
#!/usr/bin/env node
```
at the top.

##### Refer: Section "Custom packages Created and Published" for already published packages

#### 4. Update package.json

IMPORTANT:

Your "bin" must point to the COMPILED JS file, not .ts.
```json
{
  "name": "package-name",
  "version": "1.0.0",
  "type": "module",

  "bin": {
    "package-name": "./dist/index.js"
  },

  "scripts": {
    "build": "tsc"
  }
}
```

#### 5. Refer Section *Local Testing* and *Publish to npm* after all the code has been updated according to your file structure.

------------------------------------------------------------------------------------------------------------------------------------------------------------------

## Local Testing

Before publishing to npm:

- We are going to follow steps:
  
  *Step 1*: We are going to run below command to check which all folders will npm read when we publish our package. If it misses any folder we can check and update our configuration accordingly.
  ```
  npm link
  ```
  OR
  ```
  npm pack
  ```
  
  *Step 2*: Now if all the folders that we expect to go to npm is included via above cmds, then, we are going to use Verdaccio, lightweight, open-source tool used to run a private, local npm package registry.(https://verdaccio.org/)
  
  - Installation:
    
    Using npm
    ```
    npm install -g verdaccio
    ```
    or using yarn,
    ```
    yarn global add verdaccio
    ```
    or using pnpm
    ```
    pnpm install -g verdaccio
    ```
  - Once it has been installed, you only need to execute the CLI command:
    ```
    $> verdaccio
     ```
  - You can set the registry by using the following command. This will ensure that you now install the packages that are published on local registry. If they do       not exist then it will got to npmjs.org.
    ```
    npm set registry http://localhost:4873/
    ```
  - Keep the above terminal running where we ran verdaccio command and open a new terminal to add user.
    ```
    npm adduser --registry http://localhost:4873/
    ```
  - Login if you are existing user.
    ```
    npm login --registry http://localhost:4873
    OR
    npm login // Since, we are already setting our registry
    ```
  - It is in best practice that you use version like ` 1.0.0-my-temp-fix ` when you are testing your package. So, that you can keep your main version intact and       avoid messy code
  - In *.npmrc* file, set `registry=http://localhost:4873`. Include this file in .gitignore if you have public repo if you want.
  - Build:
    ```
    npm run build
    ```
  - After build run publish command inside your package to publish it to Verdaccio.
    ```
    npm publish --registry http://localhost:4873
    OR
    npm publish // Since, we are already setting our registry
    ```
  - Once you can see your package successfully published to Verdaccio, we can now install it to check in our local.
  - Create a new folder where you can install your package to test. And run the following command:
    ```
    npm create <package-name>@1.0.10-my-temp-fix3
    OR
    npm install
    ```
  - This will run the CLI in the same way when we would install from npm.
    

## Publish to npm

*Checks*: 
- Make sure your registery is set to registry.npmjs.org and not local registry like we have set for Verdaccio above:
  ```
  npm config get registry // to get the registry
  npm config set registry https://registry.npmjs.org // set this to install the package from npmjs
  ```
- In *.npmrc* file, set `registry=https://registry.npmjs.org/`.

Build:
```
npm run build
```
 
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
npm create <package-name>@latest
```

## About packages installed:
| Package  | Purpose                 |
| -------- | ----------------------- |
| fs-extra | Copy files easily       |
| chalk    | Colored terminal output |
| ora      | Spinner                 |
| prompts  | CLI questions           |
| execa    | Run shell commands      |


# Custom packages Created and Published

## 1. create-react-express-mongo
For direct installation with only limited tech stack.
- https://github.com/nimisha-527/create-react-express-mongo.git
  
## 2. create-webapp-kit
To check code on how to provide the choice between selecting tech stack.
- https://github.com/nimisha-527/create-webapp-kit
