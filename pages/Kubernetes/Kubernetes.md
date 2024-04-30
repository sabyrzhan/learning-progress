## [[Kubernetes core]]
- ## [[Kubernetes tools]]
- ## [[Kubernetes CLI commands]]
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
	- ### #Kubernetes #cheatsheet
		- To pass [[multiple params to bash command as single string]] you can use YAML multiline string using piple (`|`) for better readability. For example:
		  ```
		  apiVersion: v1
		  kind: Pod
		  metadata:
		    name: script
		  spec:
		    containers:
		      - name: script
		        image: python
		        command:
		          - bash
		          - -c
		        args:
		          - |
		            echo "hello world";
		            python -c '
		            print("foo")
		            print("bar")
		            print("baz")
		            ';
		            sleep 180;
		  ```
		- Docker and Kubernetes  command instruction relations
			- [[ENTRYPOINT instruction]] -> [[command instruction]]
			- [[CMD instruction]] -> [[args instruction]]
		- [[Delete kubernetes pod  forcefully]]
		  ```
		  $ kubectl delete pod [pod_name] -n [namespace] --grace-period 0 --force
		  ```
		- Filter and format json output from #kubectl in CLI:
		  ```
		  kubectl get pods -l name=web -o=jsonpath='{.items..metadata.name}'
		  ```
		  Here `items..`  is array and applying filter over it
		- Restart deployment/pod inside k8s pod with [[Kubernetes cronjob]]:
			- Source: https://xaviergeerinck.com/2021/04/01/schedule-kubernetes-cronjob-to-restart-pods-automatically/
			  ```
			  # Apply With:
			  # kubectl apply -f deploy-cronjob.yaml
			  
			  # Validate with:
			  # kubectl get cronjob
			  
			  ---
			  kind: ServiceAccount
			  apiVersion: v1
			  metadata:
			    name: deleting-pods
			    namespace: default
			  
			  ---
			  apiVersion: rbac.authorization.k8s.io/v1
			  kind: Role
			  metadata:
			    name: deleting-pods
			    namespace: default
			  rules:
			    - apiGroups: [""]
			      resources: ["pods"]
			      verbs: ["get", "patch", "list", "watch", "delete"]
			  
			  ---
			  apiVersion: rbac.authorization.k8s.io/v1
			  kind: RoleBinding
			  metadata:
			    name: deleting-pods
			    namespace: default
			  roleRef:
			    apiGroup: rbac.authorization.k8s.io
			    kind: Role
			    name: deleting-pods
			  subjects:
			    - kind: ServiceAccount
			      name: deleting-pods
			  
			  ---
			  apiVersion: batch/v1beta1
			  kind: CronJob
			  metadata:
			    name: deleting-pod-myname
			    namespace: default
			  spec:
			    concurrencyPolicy: Forbid
			    schedule: "0 */3 * * *" # At minute 0 past every 6 hours
			    jobTemplate:
			      spec:
			        backoffLimit: 2
			        activeDeadlineSeconds: 600
			        template:
			          spec:
			            serviceAccountName: deleting-pods
			            restartPolicy: Never
			            containers:
			              - name: kubectl
			                image: bitnami/kubectl
			                command: [ "/bin/sh", "-c" ]
			                args: 
			                  - 'kubectl delete pod $(kubectl get pod -l app=<your_label> -o jsonpath="{.items[0].metadata.name}")'
			  
			  ```
			- Another article about [[Kubernetes cronjob]] https://iceburn.medium.com/easy-way-to-schedule-pod-restart-in-kubernetes-4d6ca2d9e958
- ## Contexts
  * Getting all contexts: `kubectl config get-contexts`
  * Using specific context: `kubectl config use-context <Name of context>`
- ## #Helm charts
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
	- ### [[#helm install]] vs [[#helm template]]
		- The difference between the two commands is that `helm install --dry-run` will send things to a Kubernetes cluster, but `helm template` won't.
		- Only `helm template --debug` (but neither `helm install` nor plain `helm template`) will generate the invalid yaml, which can be used for easy debugging, because the helm error messages are rarely helpful.
	- ### #helm #cheatsheet
		- For string concat use `printf` function. For example:
		  ```
		  {{-  $v := printf "%s-%s" .Values.serviceNamespace .Values.serviceTag -}}
		  ```
		- Adding dependencies (from URL or local folder) to helm chart file:
		  ```
		  # Chart.yaml
		  dependencies:
		  - name: nginx
		    version: "1.2.3"
		    repository: "https://example.com/charts"
		    alias: nginx_latest
		  - name: nginx
		    version: "1.2.3"
		    repository: "file://../dependency_chart/nginx"
		    alias: nginx_local
		  ```
	-
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
- ## Persistent Volumes (PV)
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