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
