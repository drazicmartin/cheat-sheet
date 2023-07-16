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
kubectl run hello-world

kubectl cluster-info

kubectl get nodes
```
