FROM node:lts-gallium

WORKDIR /app

COPY . /app

# RUN apt-get update && apt-get install nano -y

RUN npm install

EXPOSE 5000

CMD [ "npm", "start" ]