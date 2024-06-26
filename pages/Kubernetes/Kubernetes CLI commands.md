## [[Kube API proxy]]
	- To access and make request to Kubernetes API, we can use proxy:
		- ```
		  kubectl proxy
		  ```
		- By default it listens on `127.0.0.1` network interface. By passing `--address 0.0.0.0` you can make it public
		- Without kube proxy, the API host requires CA cert verification and token manually.
		- You can also run it inside a container as service and make requests to pod directly. For example here is the sample Dockerfile:
			- ```
			  FROM alpine:3
			  RUN apk add --no-cache curl && \
			      curl -L -s https://dl.k8s.io/v1.30.0/bin/linux/amd64/kubectl \
			      	-o /usr/local/bin/kubectl && \
			      chmod +x /usr/local/bin/kubectl
			  EXPOSE 8081
			  ENTRYPOINT ["kubectl", "proxy", "--address", "0.0.0.0"]
			  ```
- ## [[Create namespace]]
	- `kubectl create namespace development`
	- Create using YAML:
		- ```
		  apiVersion: v1
		  kind: Namespace
		  metadata:
		    name: [namespace-name]
		  ```
		- `kubectl apply -f namespace_file.yaml`
- ## [[Change active namespace]]
	- Install `kubens` tools: `kubens namespace_name`
- ## [[Force delete namespace]]
	- Normal deletion: `kubectl delete namespace namespace_name`
	- Force delete method 1:
	  `kubectl delete namespace --force=true --grace-period=0 namespace_name`
	- Force delete method 2 with finalizer in YAML file:
		- Save namespace manifest from k8s:
		  `kubectl get namespace [your-namespace] -o json >tmp.json`
		- Edit the file by setting the `finalizer` value to empty list like below:
		  ```
		    {
		        "apiVersion": "v1",
		        "kind": "Namespace",
		        "metadata": {
		            "creationTimestamp": "2018-11-19T18:48:30Z",
		            "deletionTimestamp": "2018-11-19T18:59:36Z",
		            "name": "<terminating-namespace>",
		            "resourceVersion": "1385077",
		            "selfLink": "/api/v1/namespaces/<terminating-namespace>",
		            "uid": "b50c9ea4-ec2b-11e8-a0be-fa163eeb47a5"
		        },
		        "spec": {
		           "finalizers": 
		        },
		        "status": {
		            "phase": "Terminating"
		        }
		    }
		  ```
		- Run `kubectl proxy`
		- In new terminal apply the edited manifets:
		  `curl -k -H "Content-Type: application/json" -X PUT --data-binary @tmp.json http://127.0.0.1:8001/api/v1/namespaces/[your-namespace]/finalize`
		- To confirm: `kubectl get namespace`. You namespace should be deleted
	- Force delete method 2a with single command using `kube proxy`:
	  ```
	  (
	  NAMESPACE=your-rogue-namespace
	  kubectl proxy &
	  kubectl get namespace $NAMESPACE -o json |jq '.spec = {"finalizers":[]}' >temp.json
	  curl -k -H "Content-Type: application/json" -X PUT --data-binary @temp.json 127.0.0.1:8001/api/v1/namespaces/$NAMESPACE/finalize
	  )
	  ```
	- Force delete method 2b with single command using `kubectl replace --raw`:
	  ```
	  kubectl get namespace "stucked-namespace" -o json \
	    | tr -d "\n" | sed "s/\"finalizers\": \[[^]]\+\]/\"finalizers\": []/" \
	    | kubectl replace --raw /api/v1/namespaces/stucked-namespace/finalize -f -
	  ```