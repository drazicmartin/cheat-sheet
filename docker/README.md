# Main Docker cheat sheet

## Basics

### Access a container

Run command inside a container
```bash
docker exec <DOCKER_NAME | DOCKER_ID>
```

### Container Management

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

Running in interactive mode
```bash
docker run -it <IMAGE_NAME>
```

Kill a container
```bash
docker stop <DOCKER_NAME | DOCKER_ID>
```

Remove a container from `docker ps`
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

