# Bootstrapping a software project
This is an attempt to organize workflow to start a software project.

## Writing the SRS

1. Create `README.md` and write the Software Requirement Specification
    - Write title
    - Write objective
    - List <b>Functional Requirements</b>
    - List <b>Non Functional Requirements</b>
2.Create a project directory
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
touch .nvm
```
Inside `.nvm`
```text
14
```
6. Install Dev dependencies
```shell
npm install json-server
```
7. Create start script in root `package.json`
```json
"server": "json-server --watch server/db.json"
```
8. Update contents for `server/db.json`
9. Install client using `create-react-app`
```shell
npx create react-app client --template @chakra-ui/typescript
```
10. Remove following unused files inside `client/src`
    - logo.svg
    - Logo.tsx
11. Clean up imports and module in `App.tsx`
12. Create a `components` directory inside `client/src`


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
5. Install dependencies
```shell
npm install express dotenv
```
6. Create src directory inside root
```shell
mkdir src
```
7. Configure `tsconfig.json`
```json
"root": "src",
"outDir": "dist"
```
8. Create `includes` after `compilerOptions` in `tsconfig.json`
```json
"include": ["./src/**/*.ts"]
```
9. Add scripts to `package.json`
```json
"buid": "tsc",
"dev": "nodemon ./src/app.ts",
"start": "node ./dist/app.js"
```
10. Create the `index.ts` Express App
```ts
const app = Express()
```
11. Make sure <b>NO CORS</b> is set on Development 
```ts
App.use((req: Request, res: Response, next: NextFunction) => {
  res.header("Access-Control-Allow-Origin", "*");
  res.header(
    "Access-Control-Allow-Headers",
    "Origin, X-Requested-With, Content-Type, Accept"
  );
  next();
});
```
