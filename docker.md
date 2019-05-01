# Docker Getting Start

## Pull a image

Default pull the container image from http://hub.docker.com

Default tag: `latest`

    docker pull nginx

List container images on local machine

    docker images

Download specific version of the container image

    docker pull nginx:1.7.9

## Run a container

`-d` Running in background

`-p` Binding port, format: `LOCAL_PORT:CONTAINER_PORT`

`--name` Give container name

    docker run -d -p 8080:80 --name nginx nginx

You can launch browser and access http://127.0.0.1:8080

## List running container

    docker ps

## Remove container

`-f` Force action

`CONTAINER_ID` is showed in `docker ps`

`CONTAINER_NAME` is the container name

    docker rm -f CONTAINER_ID
    docker rm -f CONTAINER_NAME

List all containers

    docker ps -a

Just for testing 

`-i` Interactive

`-t` TTY

`--rm` Automatically remove container when container exits

`-p` Binding port, format: `LOCAL_PORT:CONTAINER_PORT`

    docker run -it --rm -p 8080:80 nginx

## Create a new image from running container

Run a container

    docker run -d -p 8080:80 --name nginx nginx

Attach new session to the container

    docker exec -it nginx bash
    # echo 'Hello World!!!' > /usr/share/nginx/html/index.html
    # exit

You can launch browser and access http://127.0.0.1:8080

See logs

    docker logs nginx

Commit change as new container image

    docker commit nginx nginx:my_v1

List running container

    docker ps

Run a container with the new image

    docker rm -f nginx
    docker run -d -p 8080:80 --name nginx_v1 nginx:my_v1

You can launch browser and access http://127.0.0.1:8080

Remove container

    docker rm -f nginx_v1

## Create container image with Dockerfile

Edit `Dockerfile`

    FROM nginx
    
    RUN echo 'Love and Peace' > /usr/share/nginx/html/index.html

Build the container image with `Dockerfile`

    docker build -t nginx:my_v2 .

List running container

    docker ps

Run a container with the new image

    docker run -d -p 8080:80 --name nginx_v2 nginx:my_v2

You can launch browser and access http://127.0.0.1:8080

Remove container

    docker rm -f nginx_v2

## Inspect Image

Install `dive`: [https://github.com/wagoodman/dive](https://github.com/wagoodman/dive)

Inspect image layers with `dive`

    dive nginx:my_v2

Rebuild image with smaller base image

Edit `Dockerfile`

    FROM nginx:alpine
    
    RUN echo 'Pineapple Juice' > /usr/share/nginx/html/index.html

Build the container image with `Dockerfile`

    docker build -t nginx:my_v3 .

Inspect image layers with `dive` again

    dive nginx:my_v3

## Mount a volume

Edit `html/ndex.html`

    Local King

Run container

    docker run -d -p 8080:80 -v $(pwd)/html:/usr/share/nginx/html --name nginx nginx

You can launch browser and access http://127.0.0.1:8080

Edit `html/ndex.html`

    Local King2

You can launch browser and access http://127.0.0.1:8080

Remove container

    docker rm -f nginx_v2

## Push image to registry

Optional 1: [Docker Hub](https://hub.docker.com/)

Optional 2: [Private Registry](https://docs.docker.com/registry/)

Run Private Registry

    docker run -d -p 5000:5000 --name registry registry:2

Tag image

    docker tag nginx:my_v2 localhost:5000/nginx:my_v2

Push Image to the Private Registry

    docker push localhost:5000/nginx:my_v2

Run container

    docker run -d -p 8080:80 --name nginx_v2 localhost:5000/nginx:my_v2

You can launch browser and access http://127.0.0.1:8080

Remove containers

    docker rm -f nginx_v2
    docker container stop registry && docker container rm -v registry

## Remove Images

Install dockly: [https://github.com/lirantal/dockly](https://github.com/lirantal/dockly)

    dockly

`m` → `Remove all Images`

    docker rmi IMAGE_NAME