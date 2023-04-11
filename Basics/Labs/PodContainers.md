
# Managing Containers with Pods

Create a nginx YAML File

```shell
  vi nginx-pod.yml
```
```shell
apiVersion: v1
kind: Pod
metadata:
  name: nginx-pod 
spec:
  containers:
  - name: nginx
  image: nginx
  ports:
  - containerPort: 80
```
Create nginx-pod.yml
```shell
kubectl create -f nginx-pod.yml
```
Check whether pod created or not using (-o wide indicates wide output)
```shell
kubectl get pods -o wide
```
To check whether we can commincate with pod
```shell
curl <IPADDRESS>of Pod
```
To view logs for container in kubernetes using
```shell
kubectl logs nginx
```
