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

## Helm charts
### Installation
Install using brew: `brew install helm`

### Cluster management
1. Create helm chart: `helm create <chart_name>`
2. Install helm chart: `helm install <chart_name> .`
3. Install helm chart with custom values: `helm install <chart_name> . -f <values_file>`
4. List installed charts: `helm list`
5. Uninstall chart: `helm uninstall <chart_name>`
6. Render config: `helm template .`
7. Render and deploy: `helm template . | kubectl apply -f -`
8. Debug errors: `helm lint`
9. Package chart: `helm package .`
10. Add repository: `helm repo add chartmuseum http://localhost:8080`
11. Search repo: `helm search repo <search>`
12. Update repo: `helm repo update`

### `chartmuseum` - charts repository
From [this](https://github.com/helm/chartmuseum) instruction:
1. Install as docker
2. Install repository: `helm repo add chartmuseum http://localhost:8080`
3. Package your chart: `helm package .`
4. Upload to repository: `curl --data-binary @<file_name> http://localhost:8080/api/charts`
5. Install chart from repository: `helm install <chart_name> chartmuseum/<chart_name>`

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
2. Create cluster (this will take ~20 minutes depending on your settings). To customise settings additional parameters can be specified. If no parameter specified, then default values will be used. For example:
```
eksctl create cluster \
    --name test-eks2 \
    --region us-east-1 \
    --nodegroup-name mainGroup \
    --node-type t3.medium \
    --nodes 2
```
3. To Delete cluster:
```
ecksctl delete cluster --name test-eks2
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
