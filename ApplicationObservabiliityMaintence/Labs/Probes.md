# Implementing Probes and Health Checks
Create a Pod with a liveness probe that checks container health by verifying that the command line responds.
```shell
vi liveness-pod.yml
```
```shell
apiVersion: v1
kind: Pod
metadata:
name: liveness-pod
spec:
containers:
- name: busybox
image: busybox:stable
command: ['sh', '-c', 'while true; do sleep 10; done']
livenessProbe:
exec:
command: ['echo', 'health check!']
initialDelaySeconds: 5
periodSeconds: 5
```
```shell
kubectl apply -f liveness-pod.yml
```
Check the status of the Pod.
```shell
kubectl get pod liveness-pod
```
You can see information about a container's liveness probes with kubectl describe .
```shell
kubectl describe pod liveness-pod
```
Create a Pod with a liveness probe and a readiness probe, both of which use an http check.
```shell
vi readiness-pod.yml
```
```shell
apiVersion: v1
kind: Pod
metadata:
name: readiness-pod
spec:
containers:
- name: nginx
image: nginx:1.20.1
ports:
- containerPort: 80
livenessProbe:
httpGet:
path: /
port: 80
initialDelaySeconds: 3
periodSeconds: 3
readinessProbe:
httpGet:
path: /
port: 80
initialDelaySeconds: 15
periodSeconds: 5
```
```shell
kubectl apply -f readiness-pod.yml
```
Check the status of the Pod. 

Initially, the status will be Running but the container will not be ready. Wait a bit and check the
Pod status again. Once the readiness probe runs successfully, the container should become ready.
```shell
kubectl get pod readiness-pod
```
