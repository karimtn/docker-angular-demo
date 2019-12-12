# Building Angular 9 with Docker container

## Create your Angular 9 project 

    ng new demo

## Create a new Docker file 

Dockerfile

    FROM node:12.6.0

    RUN wget -q -O - https://dl-ssl.google.com/linux/linux_signing_key.pub | apt-key add -
    RUN sh -c 'echo "deb [arch=amd64] http://dl.google.com/linux/chrome/deb/ stable main" >> /etc/apt/sources.list.d/google.list'
    RUN apt-get update && apt-get install -yq google-chrome-stable

    WORKDIR /app

    ENV PATH /app/node_modules/.bin:$PATH

    COPY package.json /app/package.json
    RUN npm install
    RUN npm install -g @angular/cli@latest

    COPY . /app

    CMD ng serve --host 0.0.0.0 --poll 1


## Build image container

    docker build -t demo .

## Show running container

    docker ps 

## Create a docker-compose.yml file

    version: '3.7'

    services:

        demo:
            container_name: docker_angular_demo
        build:
            context: .
            dockerfile: Dockerfile
        volumes:
            - '.:/app'
            - '/app/node_modules'
        ports:
            - '4201:4200'


## Building image container 

    docker-compose up -d --build

## Run Angular 9 with live reload

    docker-compose up
