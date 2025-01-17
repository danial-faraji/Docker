# Docker
Docker is a very popular tool introduced to make it easier for developers to create, deploy, and run applications using containers. A container is a utility provided by Docker to package and run an application in a loosely isolated environment. Containers are lightweight and contain everything needed to run an application, such as libraries and other dependencies packed by the developer during the application’s packaging process. This assures developers that their application can be run on any other machine. Here, we’re going to provide you with an ultimate Docker Cheat Sheet that will help you to learn Docker Commands easily.
This is a summary of commonly used Docker commands and their options, as well as other useful information related to Docker. It will be a handy reference for you to perform various tasks with Docker, such as installing, building, running, shipping, and cleaning up containers

## Docker Installation
- [Install Ubuntu](https://docs.docker.com/engine/install/ubuntu/)
- [Install CentOS](https://docs.docker.com/engine/install/centos/)

## Docker Login Commands:
```
# Log in to a Registry
docker login <registry>
# Login into Docker
docker login -u <username>
# Log in to a Registry
docker logout <registry>
```

## Image Management Commands:
```
# Listing Images
docker images
# Build an image
docker build -t <image_name>:<tag> <Dockerfile_path>
docker build -t myapp:v1 .
# Pulling an Image
docker image pull <name_of_image>:<tag>
docker image pull nginx:alpine
# Pull an image from a Docker Hub
docker pull <image_name>
# Pushing an image
docker image push <username_of_registry:image_name:tag>
```

## Containers Management Commands:
```
# Check the Containers
docker ps
docker ps -a
# Container Logs
docker logs <container_id>
# Starting Containers
docker container start nginx
# Stopping Containers
docker container stop nginx
# Restarting Containers
docker container restart nginx
```

## Docker Network Commands:
```
# Creating a Network
docker network create <My_Network>
# Removing a Network
docker network rm <My_Network>
# Listing Networks
docker network ls
# Getting Information About a Network
docker network inspect <My_Network>
```

## Docker Commands Removing Containers, Images, Volumes, And Networks:
```
# Removing an Image
docker rmi -f <image_name>
# Removing an Containers
docker rm -f <container_id>
# Removing a Container and its Volume
docker container rm -v <container_id>
# Removing all Images
docker image rm $(docker image ls -a -q)
# Removing all unused (containers, images, networks and volumes)
docker system prune -f
# Clean all
docker system prune -a
```

## Docker Volume Commands:
```
# Creates a named volume
docker volume create <my_volume>
# Lists the available volumes
docker volume ls
# Displays detailed information about a volume
docker volume inspect <my_volume>
# Removes one or more volumes
docker volume rm <my_volume>
# Removes all unused volumes
docker volume prune
```

## Docker CP commands:
```
# Copies files or directories from the local filesystem to the specified container
docker cp <my_file> <my_container>:</path/of/container>
# Copies files or directories from the specified container to the local filesystem
docker cp <my_container>:</path/of/container/files> </path/of/local>
```

## Dockerfile Commands:
- FROM : Specifies the base image for the build
```yaml
FROM ubuntu:latest
```
- RUN : Executes a command inside the container during build time
```yaml
RUN apt-get update && apt-get install -y curl
```
- CMD : Specifies the default command to run when the container starts
```yaml
CMD ["npm", "start"]
```
- EXPOSE : Informs Docker that the container listens on specific network ports at runtime
```yaml
EXPOSE 80/tcp
```
- ENV : Sets environment variables inside the container
```yaml
ENV NODE_ENV=production
```
- COPY : Copies files or directories from the build context into the container
```yaml
COPY app.js /usr/src/app/
```
- ADD : Similar to COPY but supports additional features like URL retrieval and decompression
```yaml
ADD https://example.com/file.tar.gz /usr/src/
```
- WORKDIR : Sets the working directory for subsequent instructions
```yaml
WORKDIR /usr/src/app
```
- ARG : Defines variables that users can pass at build-time to the builder with the docker build command
```yaml
ARG VERSION=1.0
```
- ENTRYPOINT : Configures a container to run as an executable
```yaml
ENTRYPOINT ["python", "app.py"]
```
- VOLUME : Creates a mount point and assigns it to a specified volume
```yaml
VOLUME /data
```
- USER : Sets the user or UID to use when running the image
```yaml
USER appuser
```
- LABEL : Adds metadata to an image in the form of key-value pairs
```yaml
LABEL version="1.0" maintainer="Danial"
```
- ONBUILD : Configures commands to run when the image is used as the base for another build
```yaml
ONBUILD ADD . /app/src
```

## Docker Compose
Compose is a tool for defining and running multi-container Docker applications. With Compose, you use a YAML file to configure your application’s services. Then, with a single command, you create and start all the services from your configuration.

#### Basic config example
- docker-compose.yml :
```yaml
version: '3'
services:
  web:
    build: .
    # build from Dockerfile
    context: ./Path
    dockerfile: Dockerfile
    ports:
     - "5000:5000"
    volumes:
     - .:/code
  redis:
    image: redis
```

#### Building
```yaml
web:
  # build from Dockerfile
  build: .
  args:     # Add build arguments
    APP_HOME: app
  # build from custom Dockerfile
  build:
    context: ./dir
    dockerfile: Dockerfile.dev
  # build from image
  image: ubuntu:14.04
```

#### Environment variables
```yaml
  # environment vars
  environment:
    RACK_ENV: development
  environment:
    - RACK_ENV=development
  # environment vars from file
  env_file: .env
  env_file: [.env, .development.env]
```

#### Ports
```yaml
  ports:
    - "3000"
    - "8000:80"  # host:container
  # expose ports to linked services (not to host)
  expose: ["3000"]
```

#### Dependencies
```yaml
  # makes the `db` service available as the hostname `database`
  # (implies depends_on)
  links:
    - db:database
    - redis
  # make sure `db` is alive before starting
  depends_on:
    - db
  # make sure `db` is healty before starting
  # and db-init completed without failure
  depends_on:
    db:
      condition: service_healthy
    db-init:
      condition: service_completed_successfully
```

#### Commands
```yaml
  # command to execute
  command: bundle exec thin -p 3000
  command: [bundle, exec, thin, -p, 3000]
  # override the entrypoint
  entrypoint: /app/start.sh
  entrypoint: [php, -d, vendor/bin/phpunit]
```

#### Other options
```yaml
  # make this service extend another
  extends:
    file: common.yml  # optional
    service: webapp
  volumes:
    - /var/lib/mysql
    - ./_data:/var/lib/mysql
  # automatically restart container
  restart: unless-stopped
  # always, on-failure, no (default)
```

#### Volume
```yaml
# mount host paths or named volumes, specified as sub-options to a service
  db:
    image: postgres:latest
    volumes:
      - "/var/run/postgres/postgres.sock:/var/run/postgres/postgres.sock"
      - "dbdata:/var/lib/postgresql/data"

volumes:
  dbdata:
```
### Advanced features
#### Labels
```yaml
services:
  web:
    labels:
      com.example.description: "Accounting web app"
```

#### Healthcheck
```yaml
are service healthy when `test` command succeed
    healthcheck:
      test: ["CMD", "curl", "-f", "http://localhost"]
      interval: 1m30s
      timeout: 10s
      retries: 3
      start_period: 40s
```

#### DNS servers
```yaml
services:
  web:
    dns: 8.8.8.8
    dns:
      - 8.8.8.8
      - 8.8.4.4
```

#### Devices
```yaml
services:
  web:
    devices:
    - "/dev/ttyUSB0:/dev/ttyUSB0"
```

#### Network
```yaml
# creates a custom network called `frontend`
networks:
  frontend:
```

#### External network
```yaml
# join a pre-existing network
networks:
  default:
    external:
      name: frontend
```

#### External links
```yaml
services:
  web:
    external_links:
      - redis_1
      - project_db_1:mysql
```

#### Hosts
```yaml
services:
  web:
    extra_hosts:
      - "somehost:192.168.1.100"
```

#### User
```yaml
# specifying user
user: root
# specifying both user and group with ids
user: 0:0
```

#### Common commands
```yaml
# Starts existing containers for a service.
docker-compose start

# Stops running containers without removing them.
docker-compose stop

# Pauses running containers of a service.
docker-compose pause

# Unpauses paused containers of a service.
docker-compose unpause

# Lists containers.
docker-compose ps

# Builds, (re)creates, starts, and attaches to containers for a service.
docker-compose up

# Stops containers and removes containers, networks, volumes, and images created by up.
docker-compose down
```

#### References
Based off document from [docker_source](https://devhints.io/docker-compose) and [docker_compose_source](https://www.geeksforgeeks.org/docker-cheat-sheet)

