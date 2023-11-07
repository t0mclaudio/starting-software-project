# Bootstrapping using JSON-SERVER
1. Install Dev dependencies
```shell
npm install -D json-server @types/json-server
```
2. Create start script in root `package.json`
```json
"server": "json-server --watch server/db.json --port 9000"
```
3. Update contents for `server/db.json`
