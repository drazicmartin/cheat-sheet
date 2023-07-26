# Kubernets 

- [Introduction](#intro)
- [kubetct](#kubetctl)
- [Pods](#pods)
- [ReplicaSet](#replicaset)
- [Deployment](#deployment)

## Quick

- [All Alias](https://gist.github.com/doevelopper/ff4a9a211e74f8a2d44eb4afb21f0a38)
- ```bash
  kubectl get all
  ```
  

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

Kubernetes use yaml or json format to describe configuration file.

Tow different nodes
| Master Node | Worker Node |
| :-: | :-: |
| Control woker nodes | Run containers |
| kube-apiserver <br> etcd <br> Controler <br> scheduler | kubelet <br> **Container runtime** |

<a name="kubectl"/>

## kubeclt

### Resources
| Name | Full | Alias |
| :-: | :-: | :-: |
| Pod | pod | po |
| Nodes | node | TODO |
| ReplicaSet | replicaset | rs |
| Deployment | deployment | deploy |

Create a pod with a docker image
```bash
kubectl run <POD_NAME> --image=<IMAGE_NAME>
```

Get Cluster info :
```bash
kubectl cluster-info
```

Get list of `resource`
```bash
kubectl get `resource`
# For more info use
kubectl get `resource` -o wide
# Get all resources at once
kubectl get all
```

Get info on `resource`
```bash
kubectl describe `resource` <RESOURCE_NAME>
```

Delete a `resource`
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

Start a pods with a yaml definition
```bash
kubectl create -f <YAML_FILE>
```

If YAML is update use this to apply change
```bash
kubectl apply -f <YAML_FILE>
```

Pod configuration are structured as follow 
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

<a name="replicaset"/>

## ReplicaSet

**DESC** : Replica Set are use to manage multiple pods at the same time.

ReplicatSet configuration are structured as follow
```bash
apiVersion: apps/v1   # See Kind and Version
kind: ReplicaSet      # See Kind and Version
metadata:
  name: myapp-replicaset
  labels:             # You can create every labels you want for example :
    app: myapp-replicaset
spec:
  selector:           # Select the Pods to managed
    matchLabels:
      app: myapp
  replicas: 3         # Number of Pods to be created
  template:           # Template of a Pod definition with label match with selector
    [POD DEFINITION]
```

Start the ReplcaSet with a yaml definition
```bash
kubectl create -f <YAML_FILE>
```

Update the definition : 3 methods
```bash
[1]
kubectl edit replicaset <REPLICASET_NAME>

[2]
kubectl scale replicaset <REPLICASET_NAME> --replicas=5
kubectl scale -f <YAML_FILE> --replicas=5

[3]
kubectl repalce -f <YAML_FILE>
```

<a name="deployment"/>

## Deployment

**DESC** : Deployment are use to manage ReplicaSet.\

Deployment configuration are structured as ReplicaSet exept for kind field
```bash
apiVersion: apps/v1   # See Kind and Version
kind: Deployment      # See Kind and Version
metadata:
  name: myapp-replicaset
  labels:             # You can create every labels you want for example :
    app: myapp-replicaset
spec:
  selector:           # Select the Pods to managed
    matchLabels:
      app: myapp
  replicas: 3         # Number of Pods to be created
  template:           # Template of a Pod definition with label match with selector
    [POD DEFINITION]
```

