# Initilializing Express & Typescript

## To Do
[] link to main page
[] create sections on initializing and deployment notes

1. Inside project directory
```shell
mkdir server
```
2. Initialize node
```shell
npm init -y
```
3. Install dependencies
```shell
npm install express dotenv cors bcryptjs express-async-handler jsonwebtoken
```
4. Install dev dependencies
```shell 
npm install -D typescript nodemon ts-node @types/express @types/node @types/cors @types/bcryptjs @types/jsonwebtoken
```
5. Generate `tsconfig.json` file
```shell
npx tsc —init
```
6. if there is an issue, try
```shell
node_modules/.bin/tsc --init
```
7. Configure `tsconfig.json`
```json
"rootDir": "src",
"outDir": "dist"
```
8. Create `includes` after `compilerOptions` in `tsconfig.json`
```json
"include": ["./src/**/*.ts"]
```
9. Create src directory inside root
```shell
mkdir src
mkdir src/routes
mkdir src/controllers
mkdir src/middlewares
mkdir src/config
```
10. Add scripts to `package.json`
```json
"build": "tsc",
"dev": "nodemon ./src/index.ts",
"start": "node ./dist/index.js"
```
11. Create the `index.ts` Express App
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
12. Create error handler in `src/middleware`
```ts
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
13. Skeleton the routes
```ts
import Express, { Router } from "express";
import { ...controllers } from "../controllers/controller";

const routes: Router = Express.Router();

routes.get("/", controller);
routes.post("/", controller);
routes.delete("/:id", controller);

export default routes;
```
14. Create controllers for routes
```ts
import { Request, Response } from "express";
import asyncHandler from "express-async-handler";

export const login = asyncHandler(async (_req: Request, res: Response) => {});
export const register = asyncHandler(async (req: Request, res: Response) => {});
```
