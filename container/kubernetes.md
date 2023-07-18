# Kubernets 

- [Introduction](#intro)
- [kubetct](#kubetctl)
- [Pods](#pods)

<a name="intro"/>

## Introductions

It is used to deploy and orchestrate containers\
It used multiple components to work :
- API server
  - front end 
- etcd
  - key value
- kubelet
  - Agent that run in each node 
- Container Runtime
  - docker for example 
- Controler
  - healty check of container
- Scheduler
  - Distributing works across

Tow different nodes
| Master Node | Worker Node |
| :-: | :-: |
| Control woker nodes | Run containers |
| kube-apiserver <br> etcd <br> Controler <br> scheduler | kubelet <br> **Container runtime** |

<a name="kubectl"/>

## kubeclt

Create a pod with a docker image
```bash
kubectl run <POD_NAME> --image=<IMAGE_NAME>
```

Get Cluster info :
```bash
kubectl cluster-info
```

Get list of **resource** : `pods` | `nodes`
```bash
kubectl get `resource`
# For more info use
kubectl get `resource` -o wide
```

Get info on **resource** : `pods` | `nodes`
```bash
kubectl describe `resource` <RESOURCE_NAME>
```

Delete a **resource** : `pods` | `nodes`
```bash
kubectl delete `resource` <RESOURCE_NAME>
```

<a name="pods"/> 

## PODS

**DESC** : pod are a single instance of an application

| Pod |
| :-: |
| Offen containe only one docker |
| We can have multiple docker that do different stuff |
| Docker in the same pod can access each other with `localhost` |

Start a pods with a yaml configuration
```bash
kubectl create -f <YAML_FILE>
```

If YAML is update use this to apply change
```bash
kubectl apply -f <YAML_FILE>
```

Pods use yaml file to describe their configuration. They implement the following keywork
```yaml
apiVersion: <VERSION> # See Kind and Version
kind: <KIND>          # See Kind and Version
metadata:
  name: <POD_NAME>
  labels:             # you can create every labels you want for example :
    app: myapp
    type: front-end
spec:
  containers:         # array of container with name and image like so
    - name: <CONTAINER_NAME>
      image: <DOCKER_IMAGE>
```

example
```yaml
apiVersion: v1        # See Kind and Version
kind: Pod             # See Kind and Version
metadata:
  name: myapp-pod
  labels:             # You can create every labels you want for example :
    app: myapp
    type : front-end
spec:
  containers:         # Array of container with name and image like so
    - name: nginx-container
      image: nginx
```

Table Kind and Version
| Kind | Version |
| :-: | :-: |
| Pod | v1 |
| Service | v1 |
| ReplicaSet | apps/v1 |
| Deployment | apps/v1 |

More on yaml configuration:

Adding environments variables
```yaml
spec:
  containers:
    - name: <CONTAINER_NAME>
      image: <DOCKER_IMAGE>
      env:            # Array of name value
        - name: <ENV_VAR_NAME>
          value: <ENV_VAR_VALUE>

```
