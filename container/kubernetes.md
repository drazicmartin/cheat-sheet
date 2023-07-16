# Kubernets 

- [Introduction](#intro)
- [kubetct](#kubetctl)

<a name="intro"/>

## Introductions

It is used to deploy and orchestrate containers

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

```bash
# Create a pod with a docker image
kubectl run <POD_NAME> --image=<IMAGE_NAME>

kubectl cluster-info

# List pods in a cluster
kubectl get nodes

# Get info on a pod
kubectl describe pod <POD_NAME>
```

## POD 

**desc** : pod are a single instance of an application

| Pod |
| :-: |
| Offen containe only one docker |
| We can have multiple docker that do different stuff |
| Docker in the same pod can access each other with `localhost` |