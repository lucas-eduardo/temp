FROM node:14.17.6-alpine

RUN apk update && apk add --update git openssh

RUN apk add -U tzdata
ENV TZ=America/Sao_Paulo
RUN cp /usr/share/zoneinfo/America/Sao_Paulo /etc/localtime

WORKDIR /app

COPY . .

ARG BACKEND_URL
ARG PROXY_PORT
ARG PORT
ARG DB_HOST
ARG FRONTEND_URL

ENV BACKEND_URL $BACKEND_URL
ENV PROXY_PORT $PROXY_PORT
ENV PORT $PORT
ENV DB_HOST $DB_HOST
ARG FRONTEND_URL $FRONTEND_URL

RUN echo "BACKEND_URL=${BACKEND_URL}" > .env
RUN echo "FRONTEND_URL=${FRONTEND_URL}" >> .env
RUN echo "PROXY_PORT=${PROXY_PORT}" >> .env
RUN echo "PORT=${PORT}" >> .env
RUN echo "DB_HOST=${DB_HOST}" >> .env

RUN npm install --silent

RUN npm run build --silent

CMD npx sequelize db:migrate \
  && node dist/server.js
