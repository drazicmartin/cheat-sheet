# Networks

Docker implement DNS server that can resolve <DOCKER_NAME> to ip adresse of the container

## 3 main networks `drivers` 

| Bridge | none | host |
| :-: | :-: | :-: |
| `docker run ubuntu` | `docker run ubuntu --network=none` | `docker run ubuntu --network=host` |
| Private network inside the docker host use | No networking | It use the docker host network |
| --port to connect from external | Can not communicate with other containers | no need to specify --port |
| multiple container can listen on the same port | Isolated network | only one container can listen on the a specific port |

Run a container on a specific network
```bash
docker run --network <NETWORK_NAME | NETWORK_ID> [...]
```

Create a new network

Usage: `docker network create [OPTIONS] NETWORK`

```bash
docker network create \
  --driver <DRIVER_NAME> \
  --subnet <SUB_NET>
  <NETWORK_NAME>

# exmaple
docker network create \
  --driver bridge \
  --subnet 180.1.0.0/16
  my-super-network
```

List all networks
```bash
docker network ls
```
