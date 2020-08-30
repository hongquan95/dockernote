FROM node:10-alpine

RUN mkdir -p /user/src/app

WORKDIR /user/src/app

ONBUILD COPY package.json /user/src/app/

ONBUILD RUN npm install

ONBUILD COPY . /user/src/app

CMD [ "npm", "start" ]


