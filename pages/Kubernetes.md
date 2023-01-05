- ##  [[Kubernetes Architecture]]
	- **Type:** Master/Node
	- **Master:** aka ControlPlane
	- **Components:**
		- **API Server** - serves all REST operations to manage objects in etcd
		- **etcd** - cluster store that stores cluster state
		- **KCM (Kube Controller Manager)** - component that performs actions on clusters objects by listening events from API Server
		- **Kube scheduler** - schedules newly created pods to nodes
		- **Cloud Controller Manager** - component wraps cloud provider specific logic to manage provider resources over provider API.
	- **Node Components:**
		- **Kubelet** - daemon that communicates with API Server and manages pod lifecycle
		- **KubeProxy** - network component that provides communication between pods, nods and containers
		- **CRI (Container Runtime Interface)** - abstract interface that manages containers.
## Contexts
* Getting all contexts: `kubectl config get-contexts`
* Using specific context: `kubectl config use-context <Name of context>`
## Helm charts
	- ### Installation
		- Install using brew: `brew install helm`
	- ### Cluster management
		- Create helm chart: `helm create <chart_name>`
		- Install helm chart: `helm install <chart_name> .`
		- Install helm chart with custom values: `helm install <chart_name> . -f <values_file>`
		- List installed charts: `helm list`
		- Uninstall chart: `helm uninstall <chart_name>`
		- Render config: `helm template .`
		- Render and deploy: `helm template . | kubectl apply -f -`
		- Debug errors: `helm lint`
		- Package chart: `helm package .`
		- Add repository: `helm repo add chartmuseum http://localhost:8080`
		- Remove repository: `helm repo remove <repo_name>`
		- Search repo: `helm search repo <search>`
		- Pull chart binary: `helm pull <chart>`
		- Update repo: `helm repo update`
	- ### `chartmuseum` - charts repository
		- From [this](https://github.com/helm/chartmuseum) instruction:
			- Install as docker
			- Install repository: `helm repo add chartmuseum http://localhost:8080`
			- Package your chart: `helm package .`
			- Upload to repository: `curl --data-binary @<file_name> http://localhost:8080/api/charts`
			- Install chart from repository: `helm install <chart_name> chartmuseum/<chart_name>`
- ## EKS Deployment
	- ### There are 2 ways to create EKS cluster:
		- Manually using AWS Console or CLI
		- Using `eksctl` tool
	- ### Create manually
		- #TODO
	- ### Create using `eksctl` tool
		- Install `eksctl` as described [here](https://github.com/weaveworks/eksctl). For example, with brew:
		  ```
		  brew tap weaveworks/tap
		  brew install weaveworks/tap/eksctl
		  ```
		- Create cluster (this will take ~20 minutes depending on your settings). To customise settings additional parameters can be specified. If no parameter specified, then default values will be used. For example:
		  ```
		  eksctl create cluster \
		    --name test-eks2 \
		    --region us-east-1 \
		    --nodegroup-name mainGroup \
		    --node-type t3.medium \
		    --nodes 2
		  ```
		- To Delete cluster:
		  ```
		  ecksctl delete cluster --name test-eks2
		  ```
## Persistent Volumes (PV)
	- [Kubernetes Persistent Volumes (PV) and the Persistent Volume Lifecycle](https://cloud.netapp.com/blog/kubernetes-persistent-storage-why-where-and-how)
- ## [[Kubernetes HA]]
	- ### ReplicaSet
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
	- ### Deployment
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
- ## [[How-To]]
	- ### Install kubernetes dashboard [[Kubernetes dashboard]]
	  id:: 63b6faef-48a6-4352-a4a5-aab5c2dabfb0
		- Create `nginx` certificate
		- Create `nginx` config K8s dashboard
		- Apply k8s deployment as described here [](https://github.com/kubernetes/dashboard)[https://github.com/kubernetes/dashboard](https://github.com/kubernetes/dashboard)
		  ```
		  kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v1.10.1/src/deploy/recommended/kubernetes-dashboard.yaml
		  ```
		- Execute following cmd to edit k8s service to make it accessible externally:
		  ```
		  kubectl -n kube-system edit service kubernetes-dashboard
		  ```
		- Edit settings below in config file:
		  ```
		  spec.type = NodePort
		  spec.ports[0].nodePort = 31744
		  ```
		- Save config file
		- Check on browser
		- Specify token to login