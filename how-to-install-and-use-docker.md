# How to install Docker on a Mac

- Will be using Homebrew to install Docker instead of downloading package from the internet
- Make sure Homebrew is installed, if not refer to [this guide](https://github.com/t0mclaudio/starting-software-project/blob/master/how-to-install-and-use-homebrew.md) for notes on how to use Homebrew

To install Docker on Mac; run this command
```shell
brew install --cask docker
```
Refer to Homebrew guide to differentiate between using `brew install` and `brew cask install`; but the short answer is the former installs CLI apps while the latter installs GUI apps

## How to use docker
```shell
docker --version
```
Create `Dockerfile` at the `root` of project
```shell
touch Dockerfile
```

Dockerfile 
```Dockerfile
FROM node:18-alpine
WORKDIR /app
COPY . .
RUN npm install --production
CMD ["node", "src/index.js"]
EXPOSE 3000
```

*IMPORTANT NOTE!*
the following ports should be similar
```js
//index.js || index.ts
const port = process.env.PORT || 8000;
```
```Dockerfile
#Dockerfile
EXPOSE 8000
```
```shell
#shell
docker run -dp 3001:8000 <name-of-container>
```

To run container
```shell
docker run -dp 127.0.0.1:3000:3000 <name-of-container>
```

Make sure Docker is running. At the root directory, build the image
`-t` tags name of docker image
```shell
docker build -t <name of docker image> .
```
To list all running containers
```
docker ps
```

To stop container, you can terminate in Docker GUI or run the following command in CLI
```shell
docker rm -f <the-container-id>
```

To update container, terminate existing running container first and run following command
```shell
docker build -t <name of docker image> .
```


