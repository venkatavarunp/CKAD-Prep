# Admission Control
Attempt to create a Pod that uses a Namespace that does not exist.
```shell
vi new-namespace-pod.yml
```
```shell
apiVersion: v1
kind: Pod
metadata:
name: new-namespace-pod
namespace: new-namespace
spec:
containers:
- name: busybox
image: busybox:stable
command: ['sh', '-c', 'while true; do echo Running...; sleep 5; done']
```
```shell
kubectl apply -f new-namespace-pod.yml
```
This will fail because specified Namespace, new-namespace , does not exist.

Change the API Server configuration to enable the NamespaceAutoProvision admission controller.
```shell
sudo vi /etc/kubernetes/manifests/kube-apiserver.yaml
```
Locate the --enable-admission-plugins flag, and add NamespaceAutoProvision to the list.
```shell
--enable-admission-plugins=NodeRestriction,NamespaceAutoProvision
```
When you save changes to this file, the API Server will be automatically re-created with the new settings. It may become
unavailable for a few moments during this process. Most kubectl commands will fail during this time, until the new API Server
is available.

Try to create new-namespace-pod again.
```shell
kubectl apply -f new-namespace-pod.yml
```
It should succeed this time, as the NamespaceAutoProvision admission controller will automatically handle the process of
creating the Namespace.

List the Namespaces to see the new Namespace.
```shell
kubectl get namespace
```
