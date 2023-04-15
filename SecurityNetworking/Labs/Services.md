# Exploring Services
Log in to the control plane node.

Create a server Pod.
```shell
vi service-server-pod.yml
```
```shell
apiVersion: v1
kind: Pod
metadata:
name: service-server-pod
labels:
app: service-server
spec:
containers:
- name: nginx
image: nginx:stable
ports:
- containerPort: 80
```
```shell
kubectl apply -f service-server-pod.yml
```
Create a ClusterIP Service for the Pod.
```shell
vi clusterip-service.yml
```
```shell
apiVersion: v1
kind: Service
metadata:
name: clusterip-service
spec:
type: ClusterIP
selector:
app: service-server
ports:
- protocol: TCP
port: 8080
targetPort: 80
```
```shell
kubectl apply -f clusterip-service.yml
```
Get the cluster IP address for the Service.
```shell
kubectl get svc clusterip-service
```
Use the cluster IP to test the Service.
```shell
curl <service cluster IP address>:8080
```
Now let's create a NodePort Service.
```shell
vi nodeport-service.yml
```
```shell
apiVersion: v1
kind: Service
metadata:
name: nodeport-service
spec:
type: NodePort
selector:
app: service-server
ports:
- protocol: TCP
port: 8080
targetPort: 80
nodePort: 30080
 ```
 ```shell
kubectl apply -f nodeport-service.yml
  ```
Test the NodePort Service. The external node port listens directly on the host, so we can access it using localhost .
  ```shell
curl localhost:30080
  ```
You can also use the public IP address of any of your Kubernetes nodes to access the service at http://<Node Public IP
address>:30080 .
