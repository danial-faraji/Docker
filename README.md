# Docker
Docker is a very popular tool introduced to make it easier for developers to create, deploy, and run applications using containers. A container is a utility provided by Docker to package and run an application in a loosely isolated environment. Containers are lightweight and contain everything needed to run an application, such as libraries and other dependencies packed by the developer during the application’s packaging process. This assures developers that their application can be run on any other machine. Here, we’re going to provide you with an ultimate Docker Cheat Sheet that will help you to learn Docker Commands easily.
This is a summary of commonly used Docker commands and their options, as well as other useful information related to Docker. It will be a handy reference for you to perform various tasks with Docker, such as installing, building, running, shipping, and cleaning up containers

## Docker Installation
- [Install Ubuntu](https://docs.docker.com/engine/install/ubuntu/)
- [Install CentOS](https://docs.docker.com/engine/install/centos/)

## Docker Login Commands:
<!-- Log in to a Registry -->
docker login <registry>
<!-- Login into Docker -->
docker login -u <username>
<!-- Log in to a Registry -->
docker logout <registry>

## Image Management Commands:
<!-- Listing Images -->
docker images
<!-- Build an image -->
docker build -t <image_name>:<tag> <Dockerfile_path>
docker build -t myapp:v1 .
<!-- Pulling an Image -->
docker image pull <name_of_image>:<tag>
docker image pull nginx:alpine
<!-- Pull an image from a Docker Hub -->
docker pull <image_name>
<!-- Pushing an image -->
docker image push <username_of_registry:image_name:tag>

## Containers Management Commands:
<!-- Check the Containers -->
docker ps
docker ps -a
<!-- Container Logs -->
docker logs <container_id>
<!-- Starting Containers -->
docker container start nginx
<!-- Stopping Containers -->
docker container stop nginx
<!-- Restarting Containers -->
docker container restart nginx

## Docker Network Commands:
<!-- Creating a Network -->
docker network create <My_Network>
<!-- Removing a Network -->
docker network rm <My_Network>
<!-- Listing Networks -->
docker network ls
<!-- Getting Information About a Network -->
docker network inspect <My_Network>

## Docker Commands Removing Containers, Images, Volumes, And Networks:
<!-- Removing an Image -->
docker rmi -f <image_name>
<!-- Removing an Containers -->
docker rm -f <container_id>
<!-- Removing a Container and its Volume -->
docker container rm -v <container_id>
<!-- Removing all Images -->
docker image rm $(docker image ls -a -q)
<!-- Removing all unused (containers, images, networks and volumes) -->
docker system prune -f
<!-- Clean all -->
docker system prune -a

## Docker Volume Commands:
<!-- Creates a named volume -->
docker volume create <my_volume>
<!-- Lists the available volumes -->
docker volume ls
<!-- Displays detailed information about a volume -->
docker volume inspect <my_volume>
<!-- Removes one or more volumes -->
docker volume rm <my_volume>
<!-- Removes all unused volumes -->
docker volume prune

## Docker CP commands:
<!-- Copies files or directories from the local filesystem to the specified container -->
docker cp <my_file> <my_container>:</path/of/container>
<!-- Copies files or directories from the specified container to the local filesystem -->
docker cp <my_container>:</path/of/container/files> </path/of/local>


## Dockerfile Commands:
- FROM : Specifies the base image for the build
`FROM ubuntu:latest`
- RUN : Executes a command inside the container during build time
`RUN apt-get update && apt-get install -y curl`
- CMD : Specifies the default command to run when the container starts
`CMD [“npm”, “start”]`
- EXPOSE : Informs Docker that the container listens on specific network ports at runtime
`EXPOSE 80/tcp`
- ENV : Sets environment variables inside the container
`ENV NODE_ENV=production`
- COPY : Copies files or directories from the build context into the container
`COPY app.js /usr/src/app/`
- ADD : Similar to COPY but supports additional features like URL retrieval and decompression
`ADD https://example.com/file.tar.gz /usr/src/`
- WORKDIR : Sets the working directory for subsequent instructions
`WORKDIR /usr/src/app`
- ARG : Defines variables that users can pass at build-time to the builder with the docker build command
`ARG VERSION=1.0`
- ENTRYPOINT : Configures a container to run as an executable
`ENTRYPOINT ["python", "app.py"]`
- VOLUME : Creates a mount point and assigns it to a specified volume
`VOLUME /data`
- USER : Sets the user or UID to use when running the image
`USER appuser`
- LABEL : Adds metadata to an image in the form of key-value pairs
  `LABEL version="1.0" maintainer="Danial"`
- ONBUILD : Configures commands to run when the image is used as the base for another build
  `ONBUILD ADD . /app/src`

## Docker Compose Commands:

