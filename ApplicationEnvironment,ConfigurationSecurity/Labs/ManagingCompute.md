# Managing Compute Resource Usage

Log in to the control plane node.

Create a new Namespace.
```shell
kubectl create namespace resources-test
```
Create a Pod with resource requests and limits.
```shell
vi resources-pod.yml
```
```shell
apiVersion: v1
kind: Pod
metadata:
name: resources-pod
namespace: resources-test
spec:
containers:
- name: busybox
image: busybox:stable
command: ['sh', '-c', 'while true; do echo Running...; sleep 5; done']
resources:
requests:
memory: 64Mi
cpu: 250m
limits:
memory: 128Mi
cpu: 500m
```
```shell
kubectl apply -f resources-pod.yml
```
Check the status of the new Pod.
```shell
kubectl get pods -n resources-test
```
Enable the ResourceQuota admission controller.
```shell
sudo vi /etc/kubernetes/manifests/kube-apiserver.yaml
```
Locate the --enable-admission-plugins flag and add ResourceQuota to the list.

Note: If the NamespaceAutoProvision controller is still enabled from a previous lesson, you can remove it from the list to
disable it, or leave it enabled if you wish.
```shell
- --enable-admission-plugins=NodeRestriction,ResourceQuota
```
Create a ResourceQuota.
```shell
vi resources-test-quota.yml
```
```shell
apiVersion: v1
kind: ResourceQuota
metadata:
name: resources-test-quota
namespace: resources-test
spec:
hard:
requests.memory: 128Mi
requests.cpu: 500m
limits.memory: 256Mi
limits.cpu: "1"
```
```shell
kubectl apply -f resources-test-quota.yml
```
Try to create a Pod that would exceed the memory limit quota for the Namespace.
```shell
vi too-many-resources-pod.yml
```
```shell
apiVersion: v1
kind: Pod
metadata:
name: too-many-resources-pod
namespace: resources-test
spec:
containers:
- name: busybox
image: busybox:stable
command: ['sh', '-c', 'while true; do echo Running...; sleep 5; done']
resources:
requests:
memory: 64Mi
cpu: 250m
limits:
memory: 200Mi
cpu: 500m
```
```shell
kubectl apply -f too-many-resources-pod.yml
```
This should fail, since this Pod, alongside the existing Pod, would exceed the memory limit quota for the Namespace.
