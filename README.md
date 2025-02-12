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

# Managing data

## Types of data

- **Application code:** env (build) + code (written by devs)
- **Temporary data:** stored in memory/files. i.e: user's input
- **Permanent data:** stored in database, must not be lost

## Volume

**Volumes are used to persist data**, ensuring that any data stored within them remains available to the container.

It’s just a folder on the host machine that is mounted to a container, making it accessible from within the container. Any changes made to this folder are reflected inside the container.

There are 2 kinds of volumes

- **anonymous volumes:** attached to a specific container. If we remove the container, the volume is also removed
- **named volumes:** survive even when we remove the container

To interact with it, we will use the command docker volume

```
docker run -d -p 3000:80 --rm --name feedback-app -v feedback:/app/feedback data-volume
```

## Bind mounts

It addresses the issue where changes aren't automatically reflected in Docker without rebuilding the image. It's useful for persistent, editable data, including source code updates.

```
-v $(pwd):/app
```

## **When to Use What?**

- **Use Docker Volumes** for **databases, persistent storage, backups**, and when running in production.
- **Use Bind Mounts** for **local development** when you need to sync files between the container and host system.

## Important Notes

Bind mounts **overwrite container files**, which can cause issues. For example, if `node_modules` exists inside the container but not on the host, it will be lost.

To prevent this, use an **anonymous volume**

```
docker run -v data:/app/feedback -v $(pwd):/app -v /app/node_modules
```
