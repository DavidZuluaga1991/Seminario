FROM ubuntu as linuxalpine
#FROM alpine as linuxalpine

MAINTAINER david.zuluaga@bitsamericas.com

# Install Node.js
RUN echo "LC_ALL=en_US.UTF-8" >> /etc/environment
RUN echo "LANG=en_US.UTF-8" >> /etc/environment
RUN echo "NODE_ENV=development" >> /etc/environment
RUN more "/etc/environment"

RUN apt-get update && \
    apt-get upgrade -y && \
    apt-get dist-upgrade -y \
    curl \
    build-essential \
    git \
    zip -y

RUN curl -sL https://deb.nodesource.com/setup_8.x | bash
RUN apt-get install --yes nodejs

RUN node -v
RUN npm -v
RUN npm i -g nodemon
RUN nodemon -v

RUN apt-get update && apt-get upgrade -y && apt-get autoremove -y

WORKDIR /app

ADD package*.json /app/

RUN npm install

ADD ./ /app/

ARG configuration=production

RUN npm run build -- --output-path=./dist/out --configuration $configuration
#RUN npm run build:ssr && npm run serve:ssr -- --output-path=./dist/out --configuration $configuration

FROM nginx:alpine

MAINTAINER david.zuluaga@bitsamericas.com

ADD nginx.conf /etc/nginx/nginx.conf

WORKDIR /usr/share/nginx/html

COPY --from=linuxalpine /app/dist/out/ .
