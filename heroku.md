# Run container on Heroku

Register an account on [Heroku](https://www.heroku.com)

Install [Heroku CLI Tool](https://devcenter.heroku.com/articles/heroku-cli#download-and-install)

## Create project

    mkdir heroku-test/webapp
    cd heroku-test

## Create application

Edit `webapp/config.ru`

    run Proc.new {
        [200, { "Content-Type" => "text/html" }, ["Hello World!!!\n"]]
    }

## Write Dockerfile

Edit `Dockerfile`

    FROM ruby:2.5-alpine

    RUN gem install rack

    ADD .webapp /opt/webapp
    WORKDIR /opt/webapp

    RUN adduser -D myuser
    USER myuser

    CMD rackup -o 0.0.0.0 -p $PORT   

Edit `.dockerignore`

    .git

## Directory Architecture

    .
    ├── Dockerfile
    ├── dump.rdb
    └── webapp
        └── config.ru

## Save project

    git init
    git add .
    git commit -m "init commit"

## Deploy container

Authorize cli tool

    heroku login

Create application on Heroku(A project and a git repository)

    heroku create

Authorize heroku conatiner registry

    heroku container:login

Build and Push image to heroku container registry

    heroku container:push web

Release applicaiton

    heroku container:release web

Test application

    heroku
