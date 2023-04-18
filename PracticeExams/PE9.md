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
Expose an Application with a NodePort Service
Examine the configuration of the existing Deployment:
```shell
kubectl describe deployment willow -n buffy
```
Take note of the Deployment's selector as well as the port the Pods listen on. Note this Deployment's Pods all have a label app=willow, and the container is listening on Port 80. This is the port the Service needs to use to communicate with these Pods.

Create the Service:
```shell
vi willow-svc.yml
```
Add the following to create a Service called willow-svc in the buffy Namespace:

Note: When copying and pasting code into Vim from the lab guide, first enter :set paste (and then i to enter insert mode) to avoid adding unnecessary spaces and hashes. To save and quit the file, press Escape followed by :wq. To exit the file without saving, press Escape followed by :q!.
```shell
apiVersion: v1
kind: Service
metadata:
  name: willow-svc
  namespace: buffy
spec:
  type: NodePort
  selector:
    app: willow
  ports:
  - protocol: TCP
    port: 8082
    targetPort: 80
```
Type ESC followed by :wq to save your changes.

Create the Service with kubectl apply:
```shell
kubectl apply -f willow-svc.yml
```
Get the Service's NodePort:
```shell
kubectl get service willow-svc -n buffy
```
Take note of the vale under the PORT(S) column. For example, you may see something like 8082:30406/TCP. In this example, 30406 is the external NodePort. This value may be different for you. Copy the value after 8082: (excluding the /TCP) for the next step.

Test the Service using the NodePort. Replace <node_port> with the value you just copied:
```shell
curl k8s-control:<node_port>
```
You should see the nginx Welcome page in the response.

Run the verification script to ensure you completed the objective:
```shell
~/verify.sh
```
Provide Network Access between Pods via a NetworkPolicy
Examine the Pods in the giles Namespace alongside their labels:
```shell
kubectl get pods -n giles --show-labels
```
Examine the NetworkPolicies in the Namespace:
```shell
kubectl get networkpolicy -n giles
```
You should see 5 NetworkPolicies.

Review the default-deny NetworkPolicy:
```shell
kubectl describe networkpolicy -n giles default-deny
```
The default-deny NetworkPolicy denies all traffic, by default.

Review the allow-all NetworkPolicy:
```shell
kubectl describe networkpolicy -n giles allow-all
```
The allow-all policy allows Ingress traffic to any port from any Pod.

Review the allow-api-1 NetworkPolicy:
```shell
kubectl describe networkpolicy -n giles allow-api-1
```
The allow-api-1 policy allows traffic to Port 80 and applies to the api-1 Pod.

Review the allow-api-2 NetworkPolicy:
```shell
kubectl describe networkpolicy -n giles allow-api-2
```
The allow-api-2 policy allows traffic to Port 80 and applies to the api-2 Pod.

Review the allow-api-3 NetworkPolicy:
```shell
kubectl describe networkpolicy -n giles allow-api-3
```
The allow-api-3 policy allows traffic to Port 80 and applies to the api-3 Pod.

From the above analysis, it looks like the allow-api-1 and allow-api-2 policies are the most relevant. You can use these policies to allow the required traffic by attaching the appropriate labels to the xander Pod. Edit the Pod:

Note: Do not attach a label to allow access to api-3, as this is not part of the required access for the Pod.
```shell
kubectl edit pod xander -n giles
```
Add the following labels under the metadata line at the top of the file:
```shell
metadata:
  labels:
    api-1-access: "true"
    api-2-access: "true"
```
Type ESC followed by :wq to save your changes.

Run the verification script to ensure you completed the objective:
```shell
~/verify.sh
```
