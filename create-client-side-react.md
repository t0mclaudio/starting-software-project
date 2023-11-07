# Create Client Side ReactJS

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
