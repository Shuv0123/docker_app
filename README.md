# Contenarising Node app 
## Dockerfile
- A Dockerfile is a text document that contains the instructions to assemble a Docker image.
-  When we tell Docker to build our image by executing the docker build command, Docker reads these instructions, executes them, and creates a Docker image as a result.

eg. Dockerfile for nginx
```bash 
# create a dockerfile to build castomised docker image

# choose a base image 
FROM nginx
# label the image
#LABLE MAINTAINER=SHUVO
#migrate the date from local host to our image/container
COPY profile /usr/share/nginx/html

#expose 80 
EXPOSE 80
# launch the app run this command at the end
CMD ["nginx", "-g", "daemon off;"]
```
## Dockerfile - app
- link https://docs.docker.com/language/nodejs/build-images/

When creating a Dockerfile for the app, we have to specify Docker what base image we would like to use for our application:
- Docker images can be inherited from other images. Therefore, instead of creating our own base image, we’ll use the official Node.js image that already has all the tools and packages that we need to run a Node.js application.
-  This is similar to class inheritance in object oriented programming. 

- `FROM node`

Next is to create a working directory:
- This instructs Docker to use this path as the default location for all subsequent commands. This way we do not have to type out full file paths but can use relative paths based on the working directory.

- `usr/src/app`

```bash
FROM node
# Set working directory
WORKDIR /usr/src/app
# Copy packages
COPY package*.json ./
# Install npm
RUN npm install -g npm@latest
# Run npm
RUN npm install express
# Copy everything from current working directory over to container's current working directory
COPY . . 
# Allow port 3000
EXPOSE 3000
# Start the app
CMD ["node", "app.js"]
```

The above Dockerfile creates an imagine. However, the size of the image is very large so we do as follows to reduce the size:

```bash 
FROM node AS app 

WORKDIR /usr/src/app

COPY package*.json ./

RUN npm install -g npm@latest

RUN npm install express

COPY . .

EXPOSE 3000

CMD ["node", "app.js"]

FROM node:alpine

WORKDIR /usr/src/app

COPY package*.json ./

RUN npm install -g npm@latest

RUN npm install express

# this line of code compresses the image
COPY --from=app /usr/src/app /usr/src/app

EXPOSE 3000

CMD ["node", "app.js"]
```

