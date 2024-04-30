- Kubernetes uses RBAC to manage accesses to user resources. There are 4 resources used to manage accesses:
	- `Role` - defines resource list and accesses to them using verbs at namespace level
	- `ClusterRole` - defines resource list and accesses to them using verbs at cluster level. The difference between `Role` is that we can use for unnamespaced resources like Nodes, PresistanceVolume etc
	- `RoleBinding` - assignment of Role to target subject like users or `ServiceAccounts`. As `roleRef` we can use `Role` as well as `ClusterRole`, which lets to reuse globally created role.
	- `ClusterRoleBinding` - assignment of Role or Cluster role to target subject like users or `ServiceAccounts`. If we use `Role` then specified subjects will have access to resources in Role's namespace. But if we specify ClusterRole, all subjects will have access to resources in any namespace.
- `ServiceAccount` - is a Kubernetes resource used to assign Role or give default permission to workloads. Used as a subject in RoleBinding and ClusterRoleBinding.
- Any namespace initially has default `ServiceAccount` where they have limited access to resource API, namespace and overall cluster.
- Any container when started contains token, ca certificate and namespace names attached as a volume at path `/var/run/secrets/kubernetes.io/serviceaccount`. They can be used to make authenticated request to Kubernetes API for example using `KUBERNETES_SERVICE_*`  environment variables
- Resources like Pods, Services, ConfigMaps, Secrets etc. dont belong to any `apiGroup`, since historically initially Kubernetes didnt have any api groups. Since then they were left in `""`(empty) api groups and always used as `apiVersion: v1`. The resources and their versions can be also fetched using:
	- ```
	  kubectl api-resources
	  ```
-