# Configuring SecurityContext for Containers
Log in to the control plane node.

Create a Pod that uses some custom securityContext settings.

These settings will:
```shell
Run the container as user ID 3000.
Run the container with group ID 4000.
```
Disable privilege escalation mode on the container process.
```shell
vi securitycontext-pod.yml
```
```shell
apiVersion: v1
kind: Pod
metadata:
name: securitycontext-pod
spec:
containers:
- name: busybox
image: busybox:stable
command: ['sh', '-c', 'while true; do echo Running...; sleep 5; done']
securityContext:
runAsUser: 3000
runAsGroup: 4000
allowPrivilegeEscalation: false
readOnlyRootFilesystem: true
```
```shell
kubectl apply -f securitycontext-pod.yml
```
Check the user and group ID used by the container.
```shell
kubectl exec securitycontext-pod -- id
```
Try to write to a file inside the container.
```shell
kubectl exec securitycontext-pod -- sh -c "echo test > test.txt"
```
This will fail, since user ID 3000 has not been given permissions to write to the file.
