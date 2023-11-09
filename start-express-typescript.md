# Starting / Deploying Express & Typescript


- [ ] TODO: Create sections on starting and deployment notes

## Starting the project
Make sure that `node` and `npm` are installed using `Homebrew`

- [ ] TODO: Create notes on installing `Node` and `NPM` using `Homebrew` as well as fixing issues encountered especially when migrating to a new machine

Inside project directory
```shell
mkdir [project folder] && cd [project folder]
```
And create a file to store node version to use for project
```shell
echo "[version number]" > .nvmrc
echo "lts/*" > .nvmrc # to default to the latest LTS version
echo "node" > .nvmrc # to default to the latest version
```
Initialize a project and create the package. json file
```shell
npm init -y
```
Install package dependencies
```shell
npm install express dotenv cors bcryptjs express-async-handler jsonwebtoken
```
Install dev dependencies
```shell 
npm install -D typescript nodemon ts-node @types/express @types/node @types/cors @types/bcryptjs @types/jsonwebtoken
```
Generate `tsconfig.json` file
```shell
npx tsc -init
```
If there is an issue, try
```shell
node_modules/.bin/tsc --init
```
Configure `tsconfig.json`
```json
"rootDir": "src",
"outDir": "dist"
```
Create `includes` after `compilerOptions` in `tsconfig.json`
```json
"include": ["./src/**/*.ts"]
```
Create src directory inside root
```shell
mkdir src
mkdir src/routes
mkdir src/controllers
mkdir src/middlewares
mkdir src/config
```
Add scripts to `package.json`
```json
"build": "tsc",
"dev": "nodemon ./src/index.ts",
"start": "node ./dist/index.js"
```
Create the `index.ts` Express App
```ts
import Express, { Application } from "express";
import cors from 'cors';
//import routes and errorHandler middleware

const app: Application = Express();
const port = process.env.PORT || 8000;

// parse application/json
app.use(Express.json());

app.use(cors());

// routes
// app.use(path, routes)

//Error handler middleware
// app.use(errorHandler);

app.listen(port, () => {
  console.log(`Server is running on port ${port}`);
});
```
Create error handler in `src/middleware`
```ts
// make filename with errorHandler.ts
import { Request, Response, NextFunction } from "express";

interface ResponseError extends Error {
  statusCode?: number;
}

export const errorHandler = (
  err: ResponseError,
  req: Request,
  res: Response,
  next: NextFunction
) => {
  const statusCode = err.statusCode ? err.statusCode : 500;
  res.status(statusCode).json({
    message: err.message,
  });
};

```
Skeleton the routes
```ts
// make filename with [name]Routes.ts
import Express, { Router } from "express";

// Replace controllers with proper namespace
import { ...[controllers exported] } from "../controllers/[controller file name]";

const routes: Router = Express.Router();

routes.get("/", controller); // replace with controller name
routes.post("/", controller); // replace with controller name
routes.delete("/:id", controller); // replace with controller name

export default routes;
```
Create controllers for routes
```ts
// make filename with [name]Controller.ts
import { Request, Response } from "express";
import asyncHandler from "express-async-handler";

export const login = asyncHandler(async (_req: Request, res: Response) => {}); // Add logic inside
export const register = asyncHandler(async (req: Request, res: Response) => {}); // Add logic inside
```

Execute the project using
```shell
npm run dev
```
