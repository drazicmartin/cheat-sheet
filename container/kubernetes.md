# Kubernets 

- [Introduction](#intro)
- [kubetct](#kubectl)
  - [Update Resource](#updateresource)
- [Pods](#pods)
- [ReplicaSet](#replicaset)
- [Deployment](#deployment)
  - [Update and Rollback](#uar)
- [Service](#service)

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

## kubectl

### Resources Type
| Name | Full | Alias |
| :-: | :-: | :-: |
| Pod | pod | po |
| Nodes | node | TODO |
| ReplicaSet | replicaset | rs |
| Deployment | deployment | deploy |
| Service | service | svc |

Table Kind and Version
| Kind | Version |
| :-: | :-: |
| Pod | v1 |
| Service | v1 |
| ReplicaSet | apps/v1 |
| Deployment | apps/v1 |

Create a pod with a docker image
```bash
kubectl run <POD_NAME> --image=<IMAGE_NAME>
```

Get Cluster info :
```bash
kubectl cluster-info
```

Get list of resource
```bash
kubectl get <RESOURCE_TYPE>
# For more info use
kubectl get <RESOURCE_TYPE> -o wide
# Get all resources at once
kubectl get all
# Get some different resources type at once
kubectl get <RESOURCE_TYPE>,<RESOURCE_TYPE>,<RESOURCE_TYPE>,...
```

Get description on a resource
```bash
Usage:
  kubectl describe (-f FILENAME | TYPE [NAME_PREFIX | -l label] | TYPE/NAME) [options]
```

Delete a resource
```bash
Usage:
  kubectl delete ([-f FILENAME] | [-k DIRECTORY] | TYPE [(NAME | -l label | --all)]) [options]
```

<a name="updateresource"/>

Update the <RESOURCE_TYPE> definition : 3 methods
1. Direct edit
   ```bash
   kubectl edit <RESOURCE_TYPE> <RESOURCE_NAME>
   ```

2. Scale
   ```bash
   kubectl scale replicaset <REPLICASET_NAME> --replicas=5
   kubectl scale -f <YAML_FILE> --replicas=5
   ```

3. YAML
   ```bash
   kubectl <replace | apply | patch> -f <YAML_FILE>
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

Adding environments variables
```yaml
spec:
  containers:
    - [...]
      env:            # Array of name value
        - name: <ENV_VAR_NAME>
          value: <ENV_VAR_VALUE>
```

<a name="replicaset"/>

## ReplicaSet

**DESC** : Replica Set are use to manage multiple pods at the same time.

ReplicatSet configuration are structured as follow
```yaml
apiVersion: apps/v1   # See Kind and Version
kind: ReplicaSet      # See Kind and Version
metadata:
  name: <REPLICATSET_NAME>
  labels:             # You can create every labels you want for example :
    <REPLICATSET_LABEL_NAME>: <REPLICATSET_LABEL_VALUE>
spec:
  selector:           # Select the Pods to managed
    matchLabels:
      <POD_LABEL_NAME>: <POD_LABEL_VALUE>
  replicas: 3         # Number of Pods to be created
  template:           # Template of a Pod definition with label match with selector
    [POD DEFINITION]
```

Start the ReplcaSet with a yaml definition
```bash
kubectl create -f <YAML_FILE>
```

<a name="deployment"/>

## Deployment

**DESC** : Deployment is a higher-level concept that manages ReplicaSets and provides declarative updates to Pods along with a lot of other useful features

Deployment configuration are structured as ReplicaSet exept for kind field
```yaml
apiVersion: apps/v1   # See Kind and Version
kind: Deployment      # See Kind and Version
metadata:
  name: <DELPOYMENT_NAME>
  labels:             # You can create every labels you want for example :
    <DELPOYMENT_LABEL_NAME>: <DEPLOYMENT_LABEL_VALUE>
spec:
  selector:           # Select the Pods to managed
    matchLabels:
      <POD_LABEL_NAME>: <POD_LABEL_VALUE>
  replicas: 3         # Number of Pods to be created
  template:           # Template of a Pod definition with label match with selector
    [POD DEFINITION]
```

<a name="uar" />

### Update and rollback

| Strategies | Description |
| :-: | :-: |
| Recreate | 1. destroy all <br> 2. recreate all |
| Rolling Update | 1. destroy and recreate one by one <br> Is the default strategie |

To Update see [Update Resource](#updateresource) with YAML

#### Manage the rollout of one or many resources.
```yaml
Valid resource types include:
  *  deployments
  *  daemonsets
  *  statefulsets
```

```yaml
Available Commands:
  history       View rollout history
  pause         Mark the provided resource as paused
  restart       Restart a resource
  resume        Resume a paused resource
  status        Show the status of the rollout
  undo          Undo a previous rollout
```

Usage:
```bash
kubectl rollout SUBCOMMAND [options]
```

Example:
```bash
# Rollback to the previous deployment
kubectl rollout undo deployment/abc

# Restart a deployment
kubectl rollout restart deployment/abc

# Restart deployments with the app=nginx label
kubectl rollout restart deployment --selector=app=nginx
```

<a name="service"/>

## Service

**DESC** : Use to manage connection between deployments

| Service Type | Description |
| :-: | :-: |
| NodePort | Made internal pod accessible from a Node through a port |
| ClusterIp | Virtual IP to have multiple pods accessible through one IP, default value of spec.type |
| Load Balancer | Load Balance the workload to multiple instance, see compatible cloud providers (gcp, aws, azure) |

All Service Type configuration are structured as follow:
```yaml
apiVersion: v1
kind: Service
metadata:
  name: <SERVICE_NAME>
spec:
  [See spec bellow]
```

- NodePort example
  ```yaml
  spec:
    type: NodePort
    port:
      - targetPort: 80
        port: 80
        nodePort: 30008
    selector:
      [LABEL_NAME]: [LABEL_VALUE]
  ```
- ClusterIP example
  ```yaml
  spec:
    type: ClusterIP
    port:
      - targetPort: 80
        port: 80
    selector:
      [LABEL_NAME]: [LABEL_VALUE]
  ```
- Load Balancer : see with your clouds providers
