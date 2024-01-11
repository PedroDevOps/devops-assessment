FROM node:14
WORKDIR /usr/src
COPY package*.json ./
RUN npm install
COPY src/ ./
EXPOSE 8080
CMD [ "node", "index.js" ]