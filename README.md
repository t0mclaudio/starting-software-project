# Bootstrapping a software project
This is an attempt to organize workflow to start a software project.

## Writing the SRS

1. Create `README.md` and write the Software Requirement Specification
    - Write title
    - Write objective
    - List <b>Functional Requirements</b>
    - List <b>Non Functional Requirements</b>
    - Perform some <b>Data estimation</b>
    
2. Create a project directory
3. Initialize NPM project
```shell 
npm init -y
```

4. Initialise git repository in root directory
```shell
git init
touch .gitignore
```
Inside `.gitignore`
```text
node_modules
.env
```
5. Set node version in project root
```shell
touch .nvmrc
```
Inside `.nvmrc`
```text
14
```
6. For mock server using JSON-SERVER, proceed to <b>Bootstrapping using JSON-SERVER</b>
7. To build a server go to <b>Bootstraping Express with Typescript</b>
8. To build a client go to <b>Bootstraping using Create React App</b>


## Bootstrapping using JSON-SERVER
1. Install Dev dependencies
```shell
npm install json-server
```
2. Create start script in root `package.json`
```json
"server": "json-server --watch server/db.json"
```
3. Update contents for `server/db.json`

## Bootstraping Express with Typescript

1. Inside project directory
```shell
mkdir server-name
```
2. Initialize node
```shell
npm init -y
```
3. Install dev dependencies
```shell 
npm install -D typescript @types/express @types/node nodemon ts-node
```
4. Generate `tsconfig.json` file
```shell
npx tsc —init
```
if there is an issue, try
```shell
node_modules/.bin/tsc --init
```
5. Install dependencies
```shell
npm install express dotenv
```
6. Configure `tsconfig.json`
```json
"rootDir": "src",
"outDir": "dist"
```
7. Create `includes` after `compilerOptions` in `tsconfig.json`
```json
"include": ["./src/**/*.ts"]
```
8. Create src directory inside root
```shell
mkdir src
```
9. Add scripts to `package.json`
```json
"build": "tsc",
"dev": "nodemon ./src/index.ts",
"start": "node ./dist/index.js"
```
10. Create the `index.ts` Express App
```ts
import Express, { Application, NextFunction, Request, Response } from "express";

const app: Application = Express();
const port = process.env.PORT || 8000;

app.use((req: Request, res: Response, next: NextFunction) => {
  res.header("Access-Control-Allow-Origin", "*");
  res.header(
    "Access-Control-Allow-Headers",
    "Origin, X-Requested-With, Content-Type, Accept"
  );
  res.header("Access-Control-Allow-Methods", "GET, POST, PUT, DELETE, OPTIONS");
  next();
});

app.get("/", (req: Request, res: Response) => {
  res.json({
    message: "Hello World",
  });
});

app.listen(port, () => {
  console.log(`Server is running on port ${port}`);
});
```

## Bootstrapping the Client Side
1. Install client using `create-react-app`
```shell
npx create-react-app client --template @chakra-ui/typescript
```
2. Remove following unused files inside `client/src`
    - logo.svg
    - Logo.tsx
3. Clean up imports and module in `App.tsx`
4. Create a `components` directory inside `client/src`
5. Install `react-router-dom` for client side routing
```shell
npm install react-router-dom
```
6. Import modules for routing
```ts
import * as React from "react";
import { Container } from "@chakra-ui/react";
import { BrowserRouter as Router, Route, Routes } from "react-router-dom";
import Main from "./pages/main";
import Login from "./pages/login";

export const App = () => (
  <Router>
    <Container>
      <Routes>
        <Route path="/" element={<Main />} />
        <Route path="/login" element={<Login />} />
      </Routes>
    </Container>
  </Router>
);
```
7. Create project directories for organisation
```shell
mkdir pages & mkdir components
```
