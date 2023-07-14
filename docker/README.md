# Main Docker cheat sheet

---
- [Global](#global)
- [Docker info](#info)
- [Access a container](#ac)
- [Docker Compose](#dc)
- [Docker Run](#dr)
- [Docker Image](#di)
  - [Dockerfile](#df)
- [Networks](/network.md)

---

<a name="global"/>

## Global

### File structure 

```tree
root
├── /var/lib/docker
│   ├── aufs
│   ├── containers
│   ├── image
│   ├── volume
```

<a name="ac"/>

## Access a container

Run command inside a container
```bash
docker exec <DOCKER_NAME | DOCKER_ID>
```

Logs Docker : `json format`
```bash
docker logs <DOCKER_NAME | DOCKER_ID>
```

Easy Ubuntu bash
```bash
docker run -it ubuntu bash
```

---

<a name="info"/>

## Docker info

Inspect Docker : `json format`
```bash
docker inspect <DOCKER_NAME | DOCKER_ID>

docker inspect <IMAGE_NAME>
```

Info in docker Host
```bash
docker info
```

Info on image
```bash
docker history <IMAGE_ID>
```

Systeme size
```bash
docker system df
```

---

<a name="dc"/>

## Docker compose

**desc** : *Docker compose is use to manage multiple containers*

 - docker compose use yaml format \
 - versions : 
   - v1 : don't use services
   - v2 & v3 : use services and are now merged

Docker compose up
```bash
docker compose up .
```

Docker build
```bash
docker compose build .
```

example from [docker-sample](https://github.com/dockersamples/example-voting-app)
```yml
# version is now using "compose spec"
# v2 and v3 are now combined!
# docker-compose v1.27+ required

services:
  vote:
    build: ./vote
    # use python rather than gunicorn for local dev
    command: python app.py
    depends_on:
      redis:
        condition: service_healthy
    healthcheck: 
      test: ["CMD", "curl", "-f", "http://localhost"]
      interval: 15s
      timeout: 5s
      retries: 3
      start_period: 10s
    volumes:
     - ./vote:/app
    ports:
      - "5000:80"
    networks:
      - front-tier
      - back-tier

  result:
    build: ./result
    # use nodemon rather than node for local dev
    entrypoint: nodemon server.js
    depends_on:
      db:
        condition: service_healthy 
    volumes:
      - ./result:/app
    ports:
      - "5001:80"
      - "5858:5858"
    networks:
      - front-tier
      - back-tier

  worker:
    build:
      context: ./worker
    depends_on:
      redis:
        condition: service_healthy 
      db:
        condition: service_healthy 
    networks:
      - back-tier

  redis:
    image: redis:alpine
    volumes:
      - "./healthchecks:/healthchecks"
    healthcheck:
      test: /healthchecks/redis.sh
      interval: "5s"
    networks:
      - back-tier

  db:
    image: postgres:15-alpine
    environment:
      POSTGRES_USER: "postgres"
      POSTGRES_PASSWORD: "postgres"
    volumes:
      - "db-data:/var/lib/postgresql/data"
      - "./healthchecks:/healthchecks"
    healthcheck:
      test: /healthchecks/postgres.sh
      interval: "5s"
    networks:
      - back-tier

volumes:
  db-data:

networks:
  front-tier:
  back-tier:
```

---

<a name="dr"/>

## Docker Run

Usage: `docker run [OPTIONS] IMAGE [COMMAND] [ARG...]`

Show running Docker
```bash
docker ps

# for extra like stopped containers
docker ps -a
```

Run a docker image by name
```bash
docker run <IMAGE_NAME>
# With a specific tag, default tag is `latest`
docker run <IMAGE_NAME>:<TAG>
```

Running in detach mode of the current bash
```bash
docker run -d <IMAGE_NAME>
# Then for reattaching
docker attach <DOCKER_NAME | DOCKER_ID>
```

Set the name of the container
```bash
docker run --name <NAME> <IMAGE_NAME>
```

Port Mapping
```bash
docker run -p <DOCKER_HOST_PORT>:<DOCKER_CONTAINER_PORT> <IMAGE_NAME>
```

Volume mapping
```bash
docker run -v <MAPPED_FOLDER>:<DOCKER_FOLDER> <IMAGE_NAME>
exmaple :
docker run -v /opt/datadir:/var/lib/mysql mysql

# or with mount
docker run \
  --mount type=bind,source:/opt/datadir,target:/var/lib/mysql \
  mysql

# or using volume name
## first create a volume
docker volume create <VOLUME_NAME>
## then map it
docker run -v <VOLUME_NAME>:<DOCKER_FOLDER> <IMAGE_NAME>
```

[DEPRECATED] Running docker with network links
```bash
docker run --name <DOCKER_NAME> --links <OTHER_DOCKER_NAME>:<OTHER_DOCKER_NAME> <IMAGE_NAME>
```

Running in Interactive Terminal mode
```bash
docker run -it <IMAGE_NAME>
```

Set envirnonment variables
```bash
docker run -e <VAR_NAME>=<VALUE>
```

Kill a container
```bash
docker stop <DOCKER_NAME | DOCKER_ID>
```

Remove a container from the local host
```bash
docker rm <DOCKER_NAME | DOCKER_ID>
```

---

<a name="di"/>

## Image

**desc** : *Images are use to create a specific container*

Pull a docker image
```bash
docker pull <IMAGE_NAME>
```

List available images on local host
```bash
docker images
```

Remove a docker image for local host
```bash
docker rmi <IMAGE_NAME>
```

<a name="df"/>

## Dockerfile : [docs](https://docs.docker.com/engine/reference/commandline/build/)

**desc** : *Dockerfile are use to create images of an application*

 - Docker image use a layered achitecture
 - Each line of the Dockerfile create a layer in the image

Dockerfile format
```bash
[INSTRUCTION] [ARGGUMENT]

# example
FROM ubuntu                                    # must include an os
RUN apt update && apt -y install python        # download python
RUN pip install django                         # install dependency
COPY . /opt/source-code                        # copy project files from host to image
ENTRYPOINT 
```

`ENTRYPOINT` : will not be override by by the input `COMMAND`

`CMD` : will be override by the input `COMMAND` in `docker run <IMAGE_NAME> <COMMAND>`

### Build Images

Usage : `docker build [OPTIONS] PATH | URL | -`

```bash
# Build an image
docker build -f Dockerfile -t <TAG_NAME> <PATH_TO_DOCKERFILE>
## --file , -f		Name of the Dockerfile (Default is PATH/Dockerfile)
## --tag , -t		Name and optionally a tag in the name:tag format

# example
docker build -f Dockerfile -t drazic/my-app .

# Push an image
docker push <DOCKER_NAME>
# example
docker push drazic/my-app
```

## Repository Private/Public

Workflow
```bash
Create a private repository
ocker run -d --name my-registry -p 5000:5000 --restart alwais registry:2

Tag image to the registry
docker image tag <IMAGE_NAME> <URL>:<PORT>/<IMAGE_NAME>

Push to the repository
docker push <URL>:<PORT>/<IMAGE_NAME>

Pull from the repository
docker pull <URL>:<PORT>/<IMAGE_NAME>
```

See all available images
```bash
curl -X GET <URL>:<PORT>/v2/_catalog
# example
curl -X GET localhost:5000/v2/_catalog
```
