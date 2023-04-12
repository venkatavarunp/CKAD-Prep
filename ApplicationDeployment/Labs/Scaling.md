# Demo on Scaling
Log in to the control plane node.

Create a Deployment.
```shell
vi nginx-deployment.yml
```
```shell
apiVersion: apps/v1
kind: Deployment
metadata:
name: nginx-deployment
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
image: nginx:1.14.2
ports:
- containerPort: 80
```
```shell
kubectl apply -f nginx-deployment.yml
```
Check the status of the Deployment.
```shell
kubectl get deployments
```
Check the Deployment's replica Pods.
```shell
kubectl get pods
```
Scale the Deployment.
```shell
kubectl scale deployment/nginx-deployment --replicas=4
```
Check the Deployment's status and replica Pods.
```shell
kubectl get deployment nginx-deployment
```
```shell
kubectl get pods
```
Another wat to Scale is by editing the Deployment spec.
```shell
kubectl edit deployment nginx-deployment
```
```shell
spec:
...
replicas: 2
```
Check the Deployment's status and replica Pods.
```shell
kubectl get deployment nginx-deployment
```
```shell
kubectl get pods
```
Copy the full name of one of the active Deployment Pods. Delete that Pod.
```shell
kubectl delete pod <Pod name> --force
```
List the Pods again. The Deployment will maintain desired state by creating a new replica to replace the deleted Pod.
```shell
kubectl get pods
```
