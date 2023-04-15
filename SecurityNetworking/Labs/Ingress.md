# Exposing Applications with Ingress
Log in to the control plane node.

Create a Pod.
```shell
vi ingress-test-pod.yml
```
```shell
apiVersion: v1
kind: Pod
metadata:
name: ingress-test-pod
labels:
app: ingress-test
spec:
containers:
- name: nginx
image: nginx:stable
ports:
- containerPort: 80
```
```shell
kubectl apply -f ingress-test-pod.yml
```
Create a ClusterIP Service for the Pod.
```shell
vi ingress-test-service.yml
```
```shell
apiVersion: v1
kind: Service
metadata:
name: ingress-test-service
spec:
type: ClusterIP
selector:
app: ingress-test
ports:
- protocol: TCP
port: 80
targetPort: 80
```
```shell
kubectl apply -f ingress-test-service.yml
```
Get the Service's cluster IP address.
```shell
kubectl get service ingress-test-service
```
Use the cluster IP address to test the Service.
```shell
curl <Service cluster IP address>
 ```
Create an Ingress.
  ```shell
vi ingress-test-ingress.yml
```
```shell
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
name: ingress-test-ingress
spec:
ingressClassName: nginx
rules:
- host: ingresstest.acloud.guru
http:
paths:
- path: /
pathType: Prefix
backend:
service:
name: ingress-test-service
port:
number: 80
```
```shell
kubectl apply -f ingress-test-ingress.yml
  ```
View a list of ingress objects.
  ```shell
kubectl get ingress
  ```
Get more details about the ingress. You should notice ingress-test-service listed as one of the backends.
  ```shell
kubectl describe ingress ingress-test-ingress
  ```
If you want to try accessing your Service through an Ingress controller, you will need to do some additional configuration.
  
First, use Helm to install the nginx Ingress controller.
  ```shell
helm repo add nginx-stable https://helm.nginx.com/stable
helm repo update
kubectl create namespace nginx-ingress
helm install nginx-ingress nginx-stable/nginx-ingress -n nginx-ingress
  ```
Get the cluster IP of the Ingress controller's Service.
  ```shell
kubectl get svc nginx-ingress-nginx-ingress -n nginx-ingress
  ```
Edit your hosts file.
  ```shell
sudo vi /etc/hosts
  ```
Use the cluster IP to add an entry to the hosts file.
  ```shell
<Nginx ingress controller Service cluster IP> ingresstest.acloud.guru
  ```
Now, test your setup.
  ```shell
curl ingresstest.acloud.guru
```
