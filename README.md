# Docker Playground

# Building an image

## What is WORKDIR?

By default, commands run in the root directory of the container. It's better to create a dedicated folder, like app, and copy your code into it. To do this, you should set the working directory to /app.

```
FROM node

# change the working directory to the app foler
WORKDIR /app

COPY . ./

RUN npm install

CMD ["node",  "server.js"]
```

## RUN vs CMD

- `RUN`: Executes a command **during the image build process**, modifying the image
- `CMD`: Specifies the default command to **run when a container starts**

## Docker Layer

In Docker, a "layer" refers to a single step in a Dockerfile, creating a distinct layer within the final image, and "caching" means that Docker stores these layers to reuse them in subsequent builds if no changes have been made to the corresponding Dockerfile instructions, significantly speeding up the image creation process by skipping unnecessary rebuilds of unchanged layers

It means it rebuilds only the changed step; otherwise, it uses the cache!

Take a look again at the above image. Each time we modify the app and rebuild, Docker unnecessarily re-runs npm install.

Here is the improvement

```
FROM node

# change the working directory to the app foler
WORKDIR /app

COPY . ./

RUN npm install

CMD ["node",  "server.js"]
```
