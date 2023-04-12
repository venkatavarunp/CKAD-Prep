# Deploying with Blue/Green and Canary Strategies

Log in to the control plane node.

Create a Deployment.
```shell
vi blue-deployment.yml
```
```shell
apiVersion: apps/v1
kind: Deployment
metadata:
name: blue-deployment
spec:
replicas: 1
selector:
matchLabels:
app: bluegreen-test
color: blue
template:
metadata:
labels:
app: bluegreen-test
color: blue
spec:
containers:
- name: nginx
image: linuxacademycontent/ckad-nginx:blue
ports:
- containerPort: 80
```
```shell
kubectl apply -f blue-deployment.yml
```
Create a Service to route traffic to the Deployment's Pods.
```shell
vi bluegreen-test-svc.yml
```
```shell
apiVersion: v1
kind: Service
metadata:
name: bluegreen-test-svc
spec:
selector:
app: bluegreen-test
color: blue
ports:
- protocol: TCP
port: 80
targetPort: 80
```
```shell
kubectl apply -f bluegreen-test-svc.yml
```
Get the Service's cluster IP address.
```shell
kubectl get service bluegreen-test-svc
```
Test the Service.
```shell
curl <bluegreen-test-svc cluster IP address>
```
Create a green Deployment.
```shell
vi green-deployment.yml
```
```shell
apiVersion: apps/v1
kind: Deployment
metadata:
name: green-deployment
spec:
replicas: 1
selector:
matchLabels:
app: bluegreen-test
color: green
template:
metadata:
labels:
app: bluegreen-test
color: green
spec:
containers:
- name: nginx
image: linuxacademycontent/ckad-nginx:green
ports:
- containerPort: 80
```
```shell
kubectl apply -f green-deployment.yml
```
Verify the status of the green deployment.
```shell
kubectl get deployment green-deployment
```
Test the green Deployment's Pod directly to verify it is working before sending user traffic to it.

First, get the IP address of the green Deployment's Pod.
```shell
kubectl get pods -o wide
```
Copy the green-deployment Pod's IP address and use it to test the 
Pod.
```shell
curl <green-deployment Pod IP address>
```
Edit the Service selector so that it points to the green Deployment's Pods.
```shell
kubectl edit service bluegreen-test-svc
```
```shell
spec:
selector:
app: bluegreen-test
color: green
```
Test the Service again. The response should indicate that you are communicating with the green version Pod.
```shell
kubectl get service bluegreen-test-svc
```
```shell
curl <bluegreen-test-svc cluster IP address>
```
### Now let's use a canary Deployment strategy. 

Create a main Deployment.
```shell
vi main-deployment.yml
```
```shell
apiVersion: apps/v1
kind: Deployment
metadata:
name: main-deployment
spec:
replicas: 3
selector:
matchLabels:
app: canary-test
environment: main
template:
metadata:
labels:
app: canary-test
environment: main
spec:
containers:
- name: nginx
image: linuxacademycontent/ckad-nginx:1.0.0
ports:
- containerPort: 80
```
```shell
kubectl apply -f main-deployment.yml
```
Create a Service.
```shell
vi canary-test-svc.yml
```
```shell
apiVersion: v1
kind: Service
metadata:
name: canary-test-svc
spec:
selector:
app: canary-test
ports:
- protocol: TCP
port: 80
targetPort: 80
```
Under `selector`, `app: canary-test` we didn't specifcally selected main environment

In blue/green strategy we used main environment. But in canary we simulataneously route the traffic between canary and main environment so we have different environments.
```shell
kubectl apply -f canary-test-svc.yml
```
Get the Service's cluster IP address.
```shell
kubectl get service canary-test-svc
```
Test the Service. At this point, you should only get responses 
from the main environment.
```shell
curl <canary-test-svc cluster IP address>
```
Create a canary Deployment.
```shell
vi canary-deployment.yml
```
```shell
apiVersion: apps/v1
kind: Deployment
metadata:
name: canary-deployment
spec:
replicas: 1
selector:
matchLabels:
app: canary-test
environment: canary
template:
metadata:
labels:
app: canary-test
environment: canary
spec:
containers:
- name: nginx
image: linuxacademycontent/ckad-nginx:canary
ports:
- containerPort: 80
```
We made sure a small portions of traffic is directed through the canary environment by controlling the `replicas` (1/4 of traffic passes through canary environment in this case).
```shell
kubectl apply -f canary-deployment.yml
```
Test the Service again. Re-run this command multiple times. You should get responses from both the main environment and the
canary environment.
```shell
curl <canary-test-svc cluster IP address>
```
