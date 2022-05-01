# Kubernetes learnings
## Architecture
Type: Master/Node
Master:
- aka ControlPlane
- Components:
  - API Server - serves all REST operations to manage objects in etcd
  - etcd - cluster store that stores cluster state
  - KCM (Kube Controller Manager) - component that performs actions on clusters objects by listening events from API Server
  - Kube scheduler - schedules newly created pods to nodes
  - Cloud Controller Manager - component wraps cloud provider specific logic to manage provider resources over provider API.
Node:
- Componenets:
  - Kubelet - daemon that communicates with API Server and manages pod lifecycle
  - KubeProxy - network component that provides communication between pods, nods and containers
  - CRI (Container Runtime Interface) - abstract interface that manages containers.

## EKS Deployment
There are 2 ways to create EKS cluster:
1. Manually using AWS Console or CLI
2. Using `eksctl` tool

## Create manually
TODO

## Create using `eksctl` tool
1. Install `eksctl` as described [here](https://github.com/weaveworks/eksctl). For example, with brew:
```
brew tap weaveworks/tap
brew install weaveworks/tap/eksctl
```
2. Create cluster. To customise settings additional parameters can be specified. If no parameter specified, then default values will be used. For example:
```
eksctl create cluster \
    --name test-eks2 \
    --region us-east-1 \
    --nodegroup-name mainGroup \
    --node-type t3.medium \
    --nodes 2
```

## K8s HA
### ReplicaSet
```
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: nginx-rs
  labels:
    app: nginx
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
        - name: nginx
          image: echo-server
          imagePullPolicy: IfNotPresent
          ports:
            - containerPort: 80
```
### Deployment
```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  labels:
    app: nginx
spec:
  replicas: 2
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
        - name: nginx
          image: echo-server
          imagePullPolicy: IfNotPresent
          ports:
            - containerPort: 80
```
