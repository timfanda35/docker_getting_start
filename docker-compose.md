# Docker Compose Getting Start

Install Docker Compose: https://docs.docker.com/compose/install/

## Create project

    mkdir -p compose/webapp
    cd compose

## Create Applicaton

Edit `webapp/config.ru`

    require 'redis'

    KEY_COUNTS='counts'
    redis = Redis.new(host: 'redis')

    run Proc.new {
        hit_counts = redis.incr(KEY_COUNTS)
        [200, { "Content-Type" => "text/html" }, ["Hit #{hit_counts}\n"]]
    }
    
Edit `webapp/Dockerfile`

    FROM ruby:2.5-alpine

    RUN gem install rack redis

    ADD . /opt/webapp
    WORKDIR /opt/webapp

    RUN adduser -D myuser
    USER myuser

    CMD rackup -o 0.0.0.0 -p $PORT

## Write Compose file

Edit `docker-compose.yaml`

    version: '3'
    services:
      web:
        build: ./webapp
        environment:
          - PORT=9292
        ports:
          - "8080:9292"
      redis:
        image: "redis:alpine"


## Run container with docker compose

The directory architecture:

    .
    ├── docker-compose.yaml
    └── webapp
        ├── Dockerfile
        └── config.ru
    
Start services

    docker-compose up

You can launch browser and access http://127.0.0.1:8080

## Run in background

    docker-compose up -d

You can launch browser and access http://127.0.0.1:8080

List running containers

    docker-compose ps

## See logs

    docker-compose logs web

## Run a command in the container

    docker-compose run redis redis-cli -h redis get counts

## Stop container

    docker-compose stop

## Remove containers include volumes

    docker-compose down --volumes
