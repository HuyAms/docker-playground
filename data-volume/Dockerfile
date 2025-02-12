FROM node:14

WORKDIR /app

COPY package.json .

RUN npm install

COPY . .

# this is anonymous volume
# we can't create a named volume with Dockerfile
# VOLUME [ "/app/feedback" ]

EXPOSE 80

CMD [ "node", "server.js" ]