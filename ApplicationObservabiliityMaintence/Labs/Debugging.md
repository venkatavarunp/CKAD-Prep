# Debugging 
Log in to the control plane node.

Create a broken Pod.
```shell
vi broken-pod.yml
```
```shell
apiVersion: v1
kind: Pod
metadata:
name: broken-pod
spec:
containers:
- name: nginx
image: nginx:1.20.q
livenessProbe:
httpGet:
path: /
port: 81
initialDelaySeconds: 3
periodSeconds: 3
```
```shell
kubectl apply -f broken-pod.yml
```
Get a list of Pods in the default namespace.
```shell
kubectl get pods
```
```shell
kubectl get pods -n default
```
Get a list of Pods in all Namespaces.
```shell
kubectl get pods --all-namespaces
```
Get more details about the Pod.
```shell
kubectl describe pod broken-pod
```
Edit the yaml to fix the Pod's image version.
```shell
vi broken-pod.yml
```
```shell
image: nginx:1.20.1
```
Delete and re-create the Pod.
```shell
kubectl delete pod broken-pod --force
```
```shell
kubectl apply -f broken-pod.yml
```
Check the Pod status again. You should notice that it begins restarting repeatedly. This is because the liveness probe is not
configured correctly.
```shell
kubectl get pod broken-pod
```
Check the Pod's container logs.
```shell
kubectl logs broken-pod
```
Check the Pod's YAML manifest.
```shell
kubectl get pod broken-pod -o yaml
```
Edit the yaml to fix the liveness probe.
```shell
vi broken-pod.yml
```
```shell
livenessProbe:
httpGet:
path: /
port: 80
```
Delete and re-create the Pod.
```shell
kubectl delete pod broken-pod --force
```
```shell
kubectl apply -f broken-pod.yml
```
Check the Pod status again.
```shell
kubectl get pod broken-pod
```
The Pod should now be working correctly.

Check the kube-apiserver logs. Note that the log file name contains a random hash. You will need to browse the file system
within /var/log/containers/ to find the file.
```shell
sudo cat /var/log/containers/
```
```shell
kube-apiserver-k8s-control_kube-system_kube-apiserver-<hash>.log
```
Check the kubelet logs.
```shell
sudo journalctl -u kubelet
```
