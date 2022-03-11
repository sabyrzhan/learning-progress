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

# K8s HA
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
