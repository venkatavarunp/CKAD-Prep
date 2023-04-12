# Rolling Updates
Log in to the control plane node.

Create a Deployment.
```shell
vi rolling-deployment.yml
```
```shell
apiVersion: apps/v1
kind: Deployment
metadata:
name: rolling-deployment
spec:
replicas: 5
selector:
matchLabels:
app: rolling
template:
metadata:
labels:
app: rolling
spec:
containers:
- name: nginx
image: nginx:1.14.2
ports:
- containerPort: 80
```
```shell
kubectl apply -f rolling-deployment.yml
```
Check the Deployment status and wait for all of the replicas to fully start up.
```shell
kubectl get deployment rolling-deployment
```
Update the Deployment's image.
```shell
kubectl set image deployment.v1.apps/rolling-deployment nginx=nginx:1.16.1
```
View the list of Pods. The Deployment's Pods will be gradually replaced with new Pods running the new image version.
```shell
kubectl get pods
```
View the status of the rollout.
```shell
kubectl rollout status deployment/rolling-deployment
```
#### You can also perform a rolling update simply by editing the Deployment manifest.
```shell
kubectl edit deployment rolling-deployment
```
Change the container image.
```shell
containers:
- name: nginx
image: nginx:1.20.1
```
Save the file to initiate the rolling update.

Check rollout status.
```shell
kubectl rollout status deployment/rolling-deployment
```
Wait for the rollout to finish, then roll back to the previous version ( 1.16.1 ).
```shell
kubectl rollout undo deployment/rolling-deployment
```
Check rollout status and the Pods.
```shell
kubectl rollout status deployment/rolling-deployment
```
```shell
kubectl get pods
```
Copy one of the pod names. Use it to check one of the pod to see that it is using the 1.16.1 image version.
```shell
kubectl describe pod <Pod name>
```
