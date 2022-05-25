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

