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
npm install -D json-server @types/json-server
```
2. Create start script in root `package.json`
```json
"server": "json-server --watch server/db.json --port 9000"
```
3. Update contents for `server/db.json`

## Bootstraping Express & Typescript

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
import Express, { Application, NextFunction, Request, Response } from "express";
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
import { ...controllers } from "../controller/controller";

const routes: Router = Express.Router();

routes.get("/", controller);
routes.post("/", controller);
routes.delete("/:id", controller);

export default routes;
```

## Configuring Mongo
1. Create app/cluster in [MongoDb cloud](www.mongodb.com)
2. Install extra packages
```shell
npm install mongoose
```
3. Add Mongo URI from MongoDB Cloud to `.env`
```
MONGO_URI={URI}
```
4. Create the db configuration inside `src/config/db.ts`
```ts
import mongoose from "mongoose";

const connectDB = async () => {
  try {
    const conn = await mongoose.connect(process.env.MONGO_URI ?? "");
    console.log("MongoDB Connected: ", conn.connection.host);
  } catch (err: any) {
    console.error(err.message);
    process.exit(1);
  }
};

export default connectDB;
```
5. Initiallise DB connection inside `src/app.ts`
```ts
import connectDB from "./config/db";

connectDB()
```
6. Create models inside `src/models`
```ts
import mongoose from "mongoose";

const TaskSchema = new mongoose.Schema(
  {
    user: {
      type: mongoose.Schema.Types.ObjectId,
      required: true,
      ref: "User",
    },
    name: {
      type: String,
      required: true,
    },
  },
  {
    timestamps: true,
  }
);

export default mongoose.model("Task", TaskSchema);
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
4. Organise project architecture
```shell
mkdir client/src/pages
mkdir client/src/components
mkdir client/src/hooks
mkdir client/src/lib
```
5. Install dependencies
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

## Using React-Query
1. Install v3 of React-Query
```shell
npm install react-query
```
2. Wrap app with react query client
```ts
import { QueryClient, QueryClientProvider } from "react-query";
const queryClient = new QueryClient();
const App = () => {
    return (
        <QueryClientProvider client={queryClient}>
        // children
        </QueryClientProvider>
    )
}
```
3. Write query
```ts
import { useQuery } from "react-query";
import { ITask } from "../components/Tasks";

async function getTasks(): Promise<ITask[]> {
  const result = await fetch("http://localhost:8000/api/tasks", {
    headers: {
      authorization: `Bearer ${localStorage.getItem("token")}`,
    },
  });
  const data = await result.json();
  return data;
}

export const useGetTasks = () => {
  return useQuery("tasks", getTasks);
};

```
