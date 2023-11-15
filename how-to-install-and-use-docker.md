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
# DEVELOPMENT ENVIRONMENT
# Use an official Node.js runtime as a base image
FROM node:18-alpine

# Set the working directory inside the container
WORKDIR /app

# Copy the application code from the host machine to the container
COPY . .

# Install Node.js dependencies based on the package.json and package-lock.json files
RUN npm install

# Expose port 8000 to the outside world (note: this is more of a documentation feature, it doesn't actually publish the port)
EXPOSE 8000

# Command to run the application when the container starts
CMD ["npm", "run", "dev"]
```

This implements a multistage process where there are two stages, namely the builder and running the production application
```Dockerfile
# PRODUCTION ENVIRONMENT
# Stage 1: Build the TypeScript application
FROM node:14 AS builder

WORKDIR /app

COPY package*.json ./

RUN npm install

COPY . .

# Build TypeScript files
RUN npm run build

# Stage 2: Create a smaller image for running the application
FROM node:14-alpine

WORKDIR /app

COPY --from=builder /app/dist ./dist
COPY package*.json ./

RUN npm install --production

CMD ["npm", "start"]

```

**IMPORTANT NOTE!**
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
# Run a Docker container in detached mode (-d) to run in the background
# Publish (map) port 8000 on the host machine to port 3000 inside the container
# Replace '<name-of-container>' with the actual name or ID of your Docker container
docker run -dp 8000:3000 <name-of-container>
```

Make sure Docker is running and in same directory of Dockerfile
```shell
# Build a Docker image using the current directory as the build context (.)
# Tag (-t) the image with a specified name ("<name-of-docker-image>")
docker build -t <name-of-docker-image> .
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


