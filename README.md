# Dockerizing a Node.js web app

## Prerequisites

* [Install Docker](https://docs.docker.com/engine/install/)

## Steps

### 1. Building your image

Go to the directory that has your `Dockerfile` and run the following command to build the Docker image. 
The `-t` flag lets you tag your image so it's easier to find later using the docker images command:

```sh
docker build . -t my-node-web-app
```

Your image will now be listed by Docker:

``` sh
docker images
```

```
# Example
REPOSITORY        TAG       IMAGE ID       CREATED         SIZE
my-node-web-app   latest    19c9f9ba0e3c   5 seconds ago   1.1GB
```

### 2. Run the image

Running your image with `-d` runs the container in detached mode, leaving the container running in the background.
The `-p` flag redirects a public port to a private port inside the container.
Run the image you previously built:

```sh
docker run -p 49160:8080 -d my-node-web-app
```

### 3. Print the output of your app

Get the container's ID:

```sh
docker ps
```

```
CONTAINER ID   IMAGE             COMMAND                  CREATED         STATUS         PORTS                                         NAMES
7c1c9b4f6457   my-node-web-app   "docker-entrypoint.s…"   2 minutes ago   Up 2 minutes   0.0.0.0:49160->8080/tcp, :::49160->8080/tcp   elastic_benz
```

Print app output with the container's ID:

```sh
docker logs 7c1c9b4f6457
```

```
Running on http://0.0.0.0:8080
```

If you need to go inside the container, you can use the exec command:

```sh
docker exec -it 7c1c9b4f6457 /bin/bash
```

```
root@7c1c9b4f6457:/usr/src/app#
```

Now, you can execute commands in the container. For example:

```
root@7c1c9b4f6457:/usr/src/app# ls -l 
total 44
-rw-r--r--  1 root root   370 Sep 22 08:08 Dockerfile
-rw-r--r--  1 root root  3431 Sep 22 09:23 README.md
drwxr-xr-x 60 root root  4096 Sep 22 08:11 node_modules
-rw-r--r--  1 root root 22276 Sep 22 08:11 package-lock.json
-rw-r--r--  1 root root   264 Sep 22 08:07 package.json
-rw-r--r--  1 root root   290 Sep 22 08:07 server.js
```

Just type the `exit` command to exit the container:

```
root@f95121aa40b6:/usr/src/app# exit
exit
```

### 4. Test your app

To test your app, get the port of your app that Docker mapped:

```sh
docker ps
```

```
CONTAINER ID   IMAGE             COMMAND                  CREATED         STATUS         PORTS                                         NAMES
7c1c9b4f6457   my-node-web-app   "docker-entrypoint.s…"   2 minutes ago   Up 2 minutes   0.0.0.0:49160->8080/tcp, :::49160->8080/tcp   elastic_benz
```

In the example above, Docker mapped the `8080` port inside of the container to the port `49160` on your machine.
Now you can call your app using curl:

```sh
curl -i localhost:49160
```

```
HTTP/1.1 200 OK
X-Powered-By: Express
Content-Type: text/html; charset=utf-8
Content-Length: 11
ETag: W/"b-Ck1VqNd45QIvq3AZd8XYQLvEhtA"
Date: Fri, 22 Sep 2023 12:24:14 GMT
Connection: keep-alive
Keep-Alive: timeout=5

Hello World
```

Install [cURL](https://fr.wikipedia.org/wiki/CURL) if needed via:

```sh
sudo apt-get install curl
```

Or use directly the file `localhost.http` of this repository with the HTTP Client of your IDE
(for example with [VSCode REST Client](https://marketplace.visualstudio.com/items?itemName=humao.rest-client) or [PHPStorm HTTP Client](https://www.jetbrains.com/help/phpstorm/http-client-in-product-code-editor.html)):

```
# localhost.http
GET http://localhost:49160
```

### 5. Shut down the image

In order to shut down the app we started, we run the `kill` command.
This uses the container's ID, which in this example was `7c1c9b4f6457`.

```sh
docker kill 7c1c9b4f6457
```

```
7c1c9b4f6457
```

Confirm that the app has stopped:

```sh
curl -i localhost:49160
```

```
curl: (7) Failed to connect to localhost port 49160: Connection refused
```

## Docker: Essential commands

### Build

Build Docker images from a Dockerfile and a "context":

```sh
docker build
```

### Images

Show all top level images, their repository and tags, and their size:

```sh
docker images
```

Remove one or more images:

```sh
docker rmi <image-id> 
```

Remove all images:

```sh
docker rmi $(docker images -q) 
```

### Containers

List all running containers:

```sh
docker ps
```

List all containers stopped, running:

```sh
docker ps -a
```

Stop the container which is running:

```sh
docker stop <container-id>
```

Stops all running containers:

```sh
docker stop $(docker ps -a -q) 
```

Start the container which is stopped:

```sh
docker start <container-id>
```

Restart the container which is running:

```sh
docker restart <container-id>
```

List port mappings of a specific container:

```sh
docker port <container-id>
```

Remove one or more stopped containers:

```sh
docker rm <container-id> 
```

Remove all stopped containers:

```sh
docker container prune  
```

```sh
docker rm $(docker ps -a -q) 
```

Remove the running container forcefully:

```sh
docker rm -f <container-id>
```

Update and stop a container that is in a crash-loop:

```sh
docker update –restart=no && docker stop 
```

Display the running processes of a container:

```sh
docker top <container-id>
```

### Exec

Connect to linux container and execute commands in container:

```sh
docker exec -it <container-id> /bin/bash
```

Bash shell with root if container is running in a different user context:

```sh
docker exec -itu <container-id> root /bin/bash
```

If bash is not available use `/bin/sh`:

```sh
docker exec -itu <container-id> /bin/sh
```

### Pull

Pull the image from docker hub repository:

```sh
docker pull <image-info>
```

### Hub

Logout from docker hub:

```sh
docker logout
```

Login to docker hub

```sh
docker login -u username -p password
```

### Stats

Display a live stream of container(s) resource usage statistics:

```sh
docker stats
```

### Version

Show the Docker version information:

```sh
docker version
```

## Resources

* https://nodejs.org/fr/docs/guides/nodejs-docker-webapp
* https://www.stacksimplify.com/aws-eks/docker-basics/docker-commands/
* https://codenotary.com/blog/extremely-useful-docker-commands

