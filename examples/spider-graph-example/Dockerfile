FROM node:12-bullseye
WORKDIR /app
COPY ./ ./
RUN npm install
RUN npm run build
EXPOSE 5601
CMD [ "node", "server.js" ]
