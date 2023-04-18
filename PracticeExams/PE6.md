## Introduction
This lab provides practice scenarios to help prepare you for the Certified Kubernetes Application Developer (CKAD) exam. You will be presented with tasks to complete, as well as server(s) and/or an existing Kubernetes cluster to complete them in. You will need to use your knowledge of Kubernetes to successfully complete the provided tasks, much like you would on the real CKAD exam. Good luck!

## Lab Environment
Use the provided environment to complete the tasks detailed in the learning objectives.

You can access all components of the cluster from the CLI server. The control plane server is k8s-control, and the worker is k8s-worker1. If you need to log in to the control plane server, for example, just use ssh k8s-control from the CLI server.

You can also use kubectl from the CLI server, control plane node, or worker to interact with the cluster. In order to use kubectl from the CLI server, you will need to select the acgk8s cluster to interact with, like so: kubectl config use-context acgk8s.

kubectl is aliased to k, and Kubernetes autocompletion is enabled. You can use the k alias like so: k get pods.

This lab includes a verification script to help you determine whether you have completed the objectives successfully. You can run the verification script with /home/cloud_user/verify.sh.

## Solution
Log in to the server using the credentials provided:
```shell
ssh cloud_user@<PUBLIC_IP_ADDRESS>
```
Switch to the acgk8s cluster context:
```shell
kubectl config use-context acgk8s
```
Create a Pod with Resource Requests
Create the Pod:
```shell
vi apple.yml
```
Create a Pod in the dev Namespace called apple. Use the nginx:stable image, and configure this Pod's container with resource requests for 256Mi memory and 250m cpu:

Note: When copying and pasting code into Vim from the lab guide, first enter :set paste (and then i to enter insert mode) to avoid adding unnecessary spaces and hashes. To save and quit the file, press Escape followed by :wq. To exit the file without saving, press Escape followed by :q!.
```shell
apiVersion: v1
kind: Pod
metadata:
  name: apple
  namespace: dev
spec:
  containers:
  - name: nginx
    image: nginx:stable
    resources:
      requests:
        memory: 256Mi
        cpu: 250m
```
Type ESC followed by :wq to save your changes.

Apply the changes with kubectl apply:
```shell
kubectl apply -f apple.yml
```
Check the Pod status:
```shell
kubectl get pod apple -n dev
```
You should see the Pod has a STATUS of Running.

Run the verification script to ensure you completed the objective:
```shell
~/verify.sh
```
Create and Consume a Secret
Get a base64-encoded string for the Secret data:
```shell
echo trustno1 | base64
```
Copy the code to use in the Secret.

Use the base64-encoded string to create the Secret:
```shell
vi secret-code.yml
```
Use the following to create a basic object with a kind of Secret called secret-code in the secure Namespace. Use the base64-encoded string that you just obtained for the code line:
```shell
apiVersion: v1
kind: Secret
metadata:
  name: secret-code
  namespace: secure
type: Opaque
data:
  code: dHJ1c3RubzEK
```
Type ESC followed by :wq to save your changes.

Apply the changes with kubectl apply:
```shell
kubectl apply -f secret-code.yml
```
Create the Pod:
```shell
vi secret-keeper.yml
```
Use the following to create a Pod called secret-keeper in the secure Namespace that runs on the busybox:latest image. Configure the container to run the command below. Provide the Secret's code key to the container as an environment variable called SECRET_STUFF:
```shell
apiVersion: v1
kind: Pod
metadata:
  name: secret-keeper
  namespace: secure
spec:
  containers:
  - name: busybox
    image: busybox:latest
    command: ['sh', '-c', 'echo $SECRET_STUFF; sleep 3600']
    env:
    - name: SECRET_STUFF
      valueFrom:
        secretKeyRef:
          name: secret-code
          key: code
```
Type ESC followed by :wq to save your changes.

Apply the changes with kubectl apply:
```shell
kubectl apply -f secret-keeper.yml
```
Check the Pod status:
```shell
kubectl get pod secret-keeper -n secure
```
You should see the Pod has a STATUS of Running.

Check the Pod's log:
```shell
kubectl logs secret-keeper -n secure
```
You should see the Secret data, trustno1, in the output.

Run the verification script to ensure you completed the objective:
```shell
~/verify.sh
```
