# Node & Docker - Dockerizing a Node.js web app

## Prerequisites

* [Install Docker](https://docs.docker.com/engine/install/)

## Steps

### 1. Building your image

Go to the directory that has your `Dockerfile` and run the following command to build the Docker image. 
The `-t` flag lets you tag your image so it's easier to find later using the docker images command:

```sh
$ docker build . -t my-node-web-app
```

Your image will now be listed by Docker:

``` sh
$ docker images
```

```
# Example
REPOSITORY                      TAG        ID              CREATED
node                            18         78b037dbb659    2 weeks ago
my-node-web-app                 latest     d64d3505b0d2    1 minute ago
```

### 2. Run the image

Running your image with `-d` runs the container in detached mode, leaving the container running in the background.
The `-p` flag redirects a public port to a private port inside the container.
Run the image you previously built:

```sh
$ docker run -p 49160:8080 -d my-node-web-app
```

## Docker: Essential commands

### Build Docker images from a Dockerfile and a "context"

```sh
$ docker build
```

### Show all top level images, their repository and tags, and their size

```sh
$ docker images
```

### List all running containers

```sh
$ docker ps
```

### List all containers stopped, running

```sh
$ docker ps -a
```

### Stop the container which is running

```sh
$ docker stop <container-id>
```

### Stops all running containers

```sh
$ docker stop $(docker ps -a -q) 
```

### Start the container which is stopped

```sh
$ docker start <container-id>
```

### Restart the container which is running

```sh
$ docker restart <container-id>
```

### List port mappings of a specific container

```sh
$ docker port <container-id>
```

### Remove the stopped container

```sh
$ docker rm <container-id> or <name>
```

### Remove the running container forcefully

```sh
$ docker rm -f <container-id> or <name>
```

### Pull the image from docker hub repository

```sh
$ docker pull <image-info>
```

### Connect to linux container and execute commands in container

```sh
$ docker exec -it <container-name> /bin/bash
```

Bash shell with root if container is running in a different user context:

```sh
$ docker exec -i -t -u root /bin/bash
```

If bash is not available use `/bin/sh`:

```sh
$ docker exec -it <container-name> /bin/sh
```

### Remove one or more images

```sh
$ docker rmi <image-id> 
```

### Remove all images

```sh
$ docker rmi $(docker images -q) 
```

### Logout from docker hub

```sh
$ docker logout
```

### Login to docker hub

```sh
$ docker login -u username -p password
```

### Display a live stream of container(s) resource usage statistics

```sh
$ docker stats
```

### Display the running processes of a container

```sh
$ docker top <container-id> or <name>
```

### Show the Docker version information

```sh
$ docker version
```

### Remove one or more containers

```sh
$ docker rm <container-id> 
```

### Remove all stopped containers

```sh
$ docker container prune  
```
or

```sh
$ docker rm $(docker ps -a -q) 
```

### Update and stop a container that is in a crash-loop

```sh
$ docker update –restart=no && docker stop 
```

## Resources

* https://nodejs.org/fr/docs/guides/nodejs-docker-webapp
* https://www.stacksimplify.com/aws-eks/docker-basics/docker-commands/
* https://codenotary.com/blog/extremely-useful-docker-commands

