FROM node

# all command would run in this directory, instead of the root
WORKDIR /app

COPY package.json ./

RUN npm install

# . means the current directory (input)
# /app means the output directory (in the container)
COPY . ./

# expose the port 80 (optional but for explicity)
EXPOSE 80

CMD ["node",  "server.js"]