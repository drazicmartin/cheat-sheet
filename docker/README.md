# Main Docker cheat sheet

## Basics

### Access a container

Run command inside a container
```bash
docker exec <DOCKER_NAME | DOCKER_ID>
```

Inspect Docker : `json format`
```bash
docker inspect <DOCKER_NAME | DOCKER_ID>
```

Logs Docker : `json format`
```bash
docker logs <DOCKER_NAME | DOCKER_ID>
```

Easy Ubuntu bash
```bash
docker run -it ubuntu bash
```

### Docker Run

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

Memory Volume mapping
```bash
docker run -v <MAPPED_FOLDER>:<DOCKER_FOLDER> <IMAGE_NAME>
exmaple :
docker run -v /opt/datadir:/var/lib/mysql mysql
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

### Image

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

## Dockerfile

## Docker file format
```bash
[INSTRUCTION] [ARGGUMENT]

# example
FROM ubuntu                                    # must include an os
RUN apt update && apt -y install python        # download python
RUN pip install django                         # install dependency
COPY . /opt/source-code                        # copy project files from host to image
ENTRYPOINT 
```

Dockerfile
```bash
# Build an image
docker build <PATH_TO_DOCKERFILE> -f Dockerfile -t <TAG_NAME>
# example
docker build . -f Dockerfile -t drazic/my-app

# Push an image
docker push <DOCKER_NAME>
# example
docker push drazic/my-app
```
