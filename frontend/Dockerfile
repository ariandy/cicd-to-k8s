FROM node:lts-gallium

ENV NODE_ENV=development

WORKDIR /app

COPY . /app

# RUN apt-get update && apt-get install nano -y 

RUN npm install

EXPOSE 3000

CMD [ "npm", "start" ]