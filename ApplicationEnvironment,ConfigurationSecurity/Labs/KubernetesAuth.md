# Understanding Kubernetes Auth
Log in to the control plane node.

Create a Role that provides permission to list Pods.
```shell
vi list-pods-role.yml
```
```shell
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
name: list-pods-role
rules:
- apiGroups: [""]
resources: ["pods"]
verbs: ["list"]
```
```shell
kubectl apply -f list-pods-role.yml
```
Create a RoleBinding to bind the new Role to the my-sa ServiceAccount. Note: This ServiceAccount was created in a previous
lesson.
```shell
vi list-pods-rb.yml
```
```shell
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
name: list-pods-rb
subjects:
- kind: ServiceAccount
name: my-sa
namespace: default
roleRef:
kind: Role
name: list-pods-role
apiGroup: rbac.authorization.k8s.io
```
```shell
kubectl apply -f list-pods-rb.yml
```
Check the logs for sa-pod .
```shell
kubectl logs sa-pod
```
Now that permissions have been provided to the ServiceAccount using RBAC, the log should indicate that the Pod is able to
successfully access the API and retrieve a list of Pods in the default Namespace.
