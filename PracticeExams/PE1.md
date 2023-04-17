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
Fix a Deployment with an Incorrect Image Name
Check the Pods in the default Namespace:
```shell
kubectl get pods
```
Note the Pods in the bruce Deployment have a status of ImagePullBackOff, indicating that this Deployment has an issue with the image name.

Edit the Deployment:

Note: When copying and pasting code into Vim from the lab guide, first enter :set paste (and then i to enter insert mode) to avoid adding unnecessary spaces and hashes. To save and quit the file, press Escape followed by :wq. To exit the file without saving, press Escape followed by :q!.
```shell
kubectl edit deployment bruce
```
Find the container image name in the Pod template, and change it from ninx:stable to nginx:stable:
```shell
      containers:
      - image: nginx:stable
```
Type ESC followed by :wq to save your changes.

Check the Pods again to see if the new Pods are working:
```shell
kubectl get pods
```
Run the verification script to ensure you completed the objective:
```shell
~/verify.sh
```
Fix an Issue with a Broken Pod
Get a list of Pods in the cave Namespace:
```shell
kubectl get pods -n cave
```
Copy the Pod's NAME.

Use the NAME of the bat Deployment's Pod to get the Pod logs:
```shell
kubectl logs <Pod NAME> -n cave
```
The Pod log should contain an error message indicating that the Pod's ServiceAccount (default) is not allowed to list the pods resource. You will need to check the existing RBAC setup in the Namespace in order to locate a ServiceAccount with the appropriate permissions.

Check the Namespace's ServiceAccounts:
```shell
kubectl get sa -n cave
```
Check the Roles in the Namespace:
```shell
kubectl get role -n cave
```
Note 2 Roles exist.

Get more detailed information on the batman-role:
```shell
kubectl describe role -n cave batman-role
```
You should see this Role has get and list permissions for deployments and services, but it doesn't have permissions for Pods.

Get more detailed information on the robin-role:
```shell
kubectl describe role -n cave robin-role
```
You should see that robin-role has permission to list Pods.

Check the RoleBindings to find out which ServiceAccount uses the robin-role:
```shell
kubectl get rolebinding -n cave
```
You should see both a batman-rb and a robin-rb. You will need to review both RoleBindings.

Get more detailed information on the batman-rb RoleBinding:
```shell
kubectl describe rolebinding -n cave batman-rb
```
This binds to the batman-role and the wayne-sa ServiceAccount. This is not the RoleBinding we need.

Get more detailed information on the robin-rb RoleBinding:
```shell
kubectl describe rolebinding -n cave robin-rb
```
You should see that robin-rb binds the robin-role to the ServiceAccount called gotham-sa. We can use this ServiceAccount to fix the Deployment.

Edit the Deployment:
```shell
kubectl edit deployment bat -n cave
```
Under the Pod template spec line, add the ServiceAccount:
```shell
  template:
    ...

    spec:
      serviceAccountName: gotham-sa
```
Type ESC followed by :wq to save your changes.

Get a list of Pods again:
```shell
kubectl get pods -n cave
```
You should see the old Pod Terminating and the new one with a Running status. Copy the name of the new Pod.

Using the NAME of the new Pod, check the new Pod's log to verify that it is now working:
```shell
kubectl logs <Pod NAME> -n cave
```
The Pod log should contain the output of a kubectl get pods that was successfully run within the Pod's container.

Run the verification script to ensure you completed the objective:
```shell
~/verify.sh
```
