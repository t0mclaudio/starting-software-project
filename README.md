# Bootstrapping a software project
This is an attempt to organize workflow to start a software project.

1. Create `README.md` and write the Software Requirement Specification
    - Write title
    - Write objective
    - List <b>Functional Requirements</b>
    - List <b>Non Functional Requirements</b>
2.Create a project directory
3. Initialize NPM project
      - `npm init -y`
5. Initialise git repository in root directory
6. Install Dev dependencies
     - `yarn add json-server`
7. Create start script in root `package.json`
    - `"server": "json-server --watch server/db.json"`
7. Update contents for 
    - `server/db.json`
8. Install client using `create-react-app`
    - `yarn create react-app client --template @chakra-ui/typescript`
9. Remove following unused files inside `client/src`
    - logo.svg
    - Logo.tsx
10. Clean up imports and module in `App.tsx`
11. Create a `components` directory inside `client/src`

Bootstrap Typescript/Express
Create proj directory

Initialize node using npm init -y

Install dev dependencies
npm install -D typescript @types/express @types/node nodemon ts-node

Generate tsconfig.json file
npx tsc —init

Install deps
npm install express dotenv


Create src directory inside root
mkdir src

Go to tsconfig
set root to src
set outDir to dist

Create includes after compilerOptions
Pass in   "include": ["./src/**/*.ts"]

Add scripts to run project
    "buid": "tsc",
    "dev": "nodemon ./src/app.ts",
    "start": "node ./dist/app.js"

Create the index.ts express app

Make sure to set CORS
