# Bootstrapping a software project
This is an attempt to organize workflow to start a software project.

## Writing the Software Requirement Specification (SRS)

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
6. For mock server using JSON-SERVER, proceed to <b>[Bootstrapping using JSON-SERVER](https://github.com/t0mclaudio/bootstrapping-app/blob/master/initializing-JSON-SERVER.md)</b>
7. To build a server go to <b>[Bootstraping Express with Typescript](https://github.com/t0mclaudio/bootstrapping-app/blob/master/initializing-express-typescript.md)</b>
8. To build a client go to <b>Bootstraping using Create React App</b>

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
mkdir src
mkdir src/pages
mkdir src/components
mkdir src/hooks
mkdir src/lib
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
