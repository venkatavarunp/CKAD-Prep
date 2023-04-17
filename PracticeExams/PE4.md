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
Change the Rollout Settings for an Existing Deployment
Open and edit the fish Deployment:
```shell
kubectl edit deployment fish
```
Under the Deployment spec, look for the Deployment strategy. Change the maxSurge value from 10% to 50% and the maxUnavailable value from 1 to 2:

Note: When copying and pasting code into Vim from the lab guide, first enter :set paste (and then i to enter insert mode) to avoid adding unnecessary spaces and hashes. To save and quit the file, press Escape followed by :wq. To exit the file without saving, press Escape followed by :q!.
```shell
  strategy:
    rollingUpdate:
      maxSurge: 50%
      maxUnavailable: 2
```
Type ESC followed by :wq to save your changes.

Run the verification script to ensure you completed the objective:
```shell
~/verify.sh
```
Perform a Rolling Update
Reopen the fish Deployment:
```shell
kubectl edit deployment fish
```
Initiate a rolling update by changing the image version of the nginx container. Change the version from 1.20.2 to 1.21.5:
```shell
      containers:
      - name: nginx
        image: nginx:1.21.5
```
Type ESC followed by :wq to save your changes.

Check the Deployment status:
```shell
kubectl get deployment fish
```
You should see 5/5 are READY.

Check the Pods:
```shell
kubectl get pods
```
You should see all 5 replicas up and running.

Run the verification script to ensure you completed the objective:
```shell
~/verify.sh
```
Roll back a Deployment to the Previous Version
Roll back the Deployment:
```shell
kubectl rollout undo deployment/fish
```
Check the Deployment status:
```shell
kubectl get deployment fish
```
You should see 5/5 as READY and 5 UP-TO-DATE, meaning the rollback was successful.

Check the Pods:
```shell
kubectl get pods
```
You should see all 5 Pods with a young age, again indicating the rollback was successful.

Run the verification script to ensure you completed the objective:
```shell
~/verify.sh
```
