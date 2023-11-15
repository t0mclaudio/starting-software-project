# Starting / Deploying Express & Typescript

- [ ] TODO: Create sections on starting and deployment notes

## General overview of stages of this guide
**1. Develop the TypeScript Application:**
- Write and develop your TypeScript application, ensuring it has proper unit tests and follows best practices.

**2. Create a Dockerfile:**
- Set up a Dockerfile with a multi-stage build approach, separating the build and runtime stages. Ensure that the Dockerfile includes all necessary dependencies, sets the working directory, and copies the required files.

**3. Setup CI/CD Workflow with GitHub Actions:**
- Set up a CI/CD workflow in GitHub Actions by creating a `.github/workflows` directory with a YAML file (e.g., ci-cd.yml).
- Define workflow triggers (e.g., on push to the production branch).
- Configure jobs to run tests, build the Docker image, and push the image to a container registry (e.g., GitHub Packages).
- Use secrets to securely store sensitive information like access tokens or credentials.
- 
**4. Use Docker Image in Production: [NOT IN SCOPE]**
- On your production server, pull the Docker image from the container registry.
- Run the Docker container using the production-ready Docker image.

**5. Monitoring and Scaling: [NOT IN SCOPE]**
- Implement monitoring tools and scaling strategies on your production server based on your application's needs.
- Ensure that your Dockerized application is set up for production with proper security configurations, environment variables, and any necessary optimizations.

### Starting the project
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
npm install express dotenv cors bcryptjs jsonwebtoken
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
Create src directory inside root
```shell
mkdir src
mkdir src/routes
mkdir src/controllers
mkdir src/middlewares
mkdir src/config
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

Don't forget to comment out routes in `index.ts`

Create controllers for routes
```ts
// make filename with [name]Controller.ts
import { Request, Response } from "express";

export const login = (_req: Request, res: Response) => {}); // Add logic inside
export const register = (req: Request, res: Response) => {}; // Add logic inside
```

**[OPTIONAL]:** Using Express Async Handler when doing async tasks like making network calls or Database calls

```shell
npm install express-async-handler
```
Add Express Async Handler to a controller
```diff
import { Request, Response } from "express";
+ import asyncHandler from "express-async-handler";

- export const login = (_req: Request, res: Response) => {}); // Add logic inside
- export const register = (req: Request, res: Response) => {}; // Add logic inside
+ export const login = asyncHandler(async (_req: Request, res: Response) => {}); // Add logic inside
+ export const register = asyncHandler(async (req: Request, res: Response) => {}); // Add logic inside
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
Don't forget to comment out error handling in index.js

Execute the project using
```shell
npm run dev
```

### Dockerize application
```shell
touch Dockerfile
```

Refer to [this guide](https://github.com/t0mclaudio/starting-software-project/blob/master/how-to-install-and-use-docker.md) to setup and run Docker container

### Setup CI/CD using Github actions
```yml
name: CI/CD

on:
  push:
    branches:
      - production

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout code
        uses: actions/checkout@v2

      - name: Set up Node.js
        uses: actions/setup-node@v4
        with:
          node-version: '14'

      - name: Install dependencies
        run: npm install

      - name: Run tests
        run: npm test

      - name: Build Docker image
        run: docker build -t ${{ secrets.DOCKER_IMAGE_NAME }} .

      - name: Push Docker image to GitHub Packages
        run: |
          echo ${{ secrets.DOCKER_TOKEN }} | docker login ghcr.io -u ${{ github.actor }} --password-stdin
          docker push ghcr.io/${{ github.repository_owner }}/${{ secrets.DOCKER_IMAGE_NAME }}
```
