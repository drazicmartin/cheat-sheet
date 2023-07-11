# Main Docker cheat sheet

## Basics

### Access docker

Run command inside a container
```bash
docker exec <DOCKER_NAME | DOCKER_ID>
```

Show running Docker
```bash
docker ps

# for extra like stopped containers
docker ps -a
```

Run a docker image by name
```bash
docker run <IMAGE_NAME>

# For running in detach mode of the current bash
docker run -d <IMAGE_NAME>
# Then for reattaching
docker attach <DOCKER_NAME | DOCKER_ID>
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

