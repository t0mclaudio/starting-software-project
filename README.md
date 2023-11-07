# Starting a software project
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
8. To build a client go to <b>[Bootstraping using Create Client Side REACTJS](https://github.com/t0mclaudio/starting-software-project/blob/master/create-client-side-react.md)</b>
9. [Add MongoDB as data store](https://github.com/t0mclaudio/starting-software-project/blob/master/adding-mongodb-as-data-store.md)
