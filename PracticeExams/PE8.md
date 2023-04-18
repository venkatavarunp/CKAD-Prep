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
Locate and Fix a Failing Liveness Probe
Check the cluster to find the broken Pod:
```shell
kubectl get pods --all-namespaces
```
You should see a broken Pod (note the CrashLoopBackOff status) in the han Namespace. Based on the Pod name, this Pod is part of the c3p0 Deployment. Copy the NAME of the Pod for the next step (it begins with c3p0-).

Create a file to save this information:

Note: When copying and pasting code into Vim from the lab guide, first enter :set paste (and then i to enter insert mode) to avoid adding unnecessary spaces and hashes. To save and quit the file, press Escape followed by :wq. To exit the file without saving, press Escape followed by :q!.
```shell
vi /home/cloud_user/probe-fix/broken-pod-name.txt
```
Use the below format to add the Pod. Replace <Pod_NAME> with the rest of the Pod name (i.e., everything after c3p0-) you copied before:
```shell
han/c3p0-<POD_NAME>
```
Type ESC followed by :wq to save your changes.

Get a list of events for the han Namespace:
```shell
kubectl get events -n han -o wide
```
You should see the full list of events. Review this list.

Save this data to the appropriate file:
```shell
kubectl get events -n han -o wide > /home/cloud_user/probe-fix/events.txt
```
Edit the Pod's Deployment:
```shell
kubectl edit deployment c3p0 -n han
```
Scroll down and note the nginx container exposes port 80, but the liveness probe seems to be checking port 1717. Change the liveness probe port to 80:
```shell
        livenessProbe:
          httpGet:
            port: 80
```
Type ESC followed by :wq to save your changes.

Check the Pod status to verify that it is now working:
```shell
kubectl get pods -n han
```
You should see the new c3p0 Pod is now Running.

Run the verification script to ensure you completed the objective:
```shell
~/verify.sh
```
Find the Pod with the Highest CPU Usage
Check CPU usage within the cpu Namespace:
```shell
kubectl top pod -n cpu
```
You should see that cpu-pod-2 is using the most CPU.

Save this Pod's name to the appropriate file:
```shell
vi /home/cloud_user/metrics/high-cpu-pod-name.txt
```
Add the following to the file:
```shell
cpu-pod-2
```
Type ESC followed by :wq to save your changes.

Run the verification script to ensure you completed the objective:
```shell
~/verify.sh
```
