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
Create a ConfigMap
Create the ConfigMap file:
```shell
vi kenobi.yml
```
Use the following to create the ConfigMap. It will be stored in the yoda Namespace with the below configuration data:

Note: When copying and pasting code into Vim from the lab guide, first enter :set paste (and then i to enter insert mode) to avoid adding unnecessary spaces and hashes. To save and quit the file, press Escape followed by :wq. To exit the file without saving, press Escape followed by :q!.
```shell
apiVersion: v1
kind: ConfigMap
metadata:
  name: kenobi
  namespace: yoda
data:
  planet: hoth
```
Type ESC followed by :wq to save your changes.

Apply the changes with kubectl apply:
```shell
kubectl apply -f kenobi.yml
```
Run the verification script to ensure you completed the objective:
```shell
~/verify.sh
```
Create a Pod That Consumes the ConfigMap as a Mounted Volume
Create the Pod:
```shell
vi chewie.yml
```
Use the following to create a Pod called chewie in the yoda Namespace, using the nginx:stable image. Provide the kenobi ConfigMap data to this Pod as a mounted volume at /etc/starwars:
```shell
apiVersion: v1
kind: Pod
metadata:
  name: chewie
  namespace: yoda
spec:
  containers:
  - name: nginx
    image: nginx:stable
    volumeMounts:
    - name: kenobi-cm
      mountPath: /etc/starwars
  volumes:
  - name: kenobi-cm
    configMap:
      name: kenobi
```
Apply the changes with kubectl apply:
```shell
kubectl apply -f chewie.yml
```
Check the Pod status to make sure it is running as expected:
```shell
kubectl get pods -n yoda
```
Run the verification script to ensure you completed the objective:
```shell
~/verify.sh
```
