FROM node:14

WORKDIR /app

COPY package.json .

RUN npm install

COPY . .

# this is anonymous volume
# we can't create a named volume with Dockerfile
# VOLUME [ "/app/feedback" ]

# arg is build time env
# we can only use in the dockerfile
ARG DEFAULT_PORT=80

ENV PORT $DEFAULT_PORT

EXPOSE $PORT

CMD [ "node", "server.js" ]