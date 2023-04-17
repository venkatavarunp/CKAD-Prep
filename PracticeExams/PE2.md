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
Configure a Deployment's Pods to Use an Existing ServiceAccount
Edit the Deployment:

Note: When copying and pasting code into Vim from the lab guide, first enter :set paste (and then i to enter insert mode) to avoid adding unnecessary spaces and hashes. To save and quit the file, press Escape followed by :wq. To exit the file without saving, press Escape followed by :q!.
```shell
kubectl edit deployment han
```
Scroll down to the Pod template and then to the Pod spec. Add the following to set the serviceAccountName to falcon-sa. You can add this right under the spec line:
```shell
  template:
    ...

    spec:
      serviceAccountName: falcon-sa
```
Type ESC followed by :wq to save your changes.

Verify the Deployment's Pods are running:
```shell
kubectl get pods
```
You should see both the han and lando Pods are up and in a Running status.

Run the verification script to ensure you completed the objective:
```shell
~/verify.sh
```
Customize Security Settings for a Deployment's Pods
Edit the Deployment:
```shell
kubectl edit deployment lando
```
Scroll down to the containers spec, and configure the securityContext for the container in the Pod template. You can add the following under the terminationMessagePolicy line:
```shell
        securityContext:
          allowPrivilegeEscalation: true
          runAsUser: 2727
```
Type ESC followed by :wq to save your changes.

Verify the Deployment's Pods are running:
```shell
kubectl get pods
```
You should see the old lando Pod Terminating and the new one in a Running status.

Run the verification script to ensure you completed the objective:
```shell
~/verify.sh
```
