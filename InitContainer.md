# Init Containers
Log in to the control plane node.

Create a Pod with an Init container that delays startup by 60 seconds.
```shell
vi init-test.yml
```
```shell
apiVersion: v1
kind: Pod
metadata:
  name: init-test
spec:
  containers:
  - name: nginx
    image: nginx:stable
  initContainers:
  - name: busybox
    image: busybox:stable
    command: ['sh', '-c', 'sleep 60']
```
```shell
kubectl apply -f init-test.yml
```
Check the Pod status.
```shell
kubectl get pod init-test
```
Wait 60 seconds or so for the init container to complete, then check the status again.
```shell
kubectl get pod init-test
```
