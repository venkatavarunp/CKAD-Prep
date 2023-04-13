# Monitoring Logs

Log in to the control plane node.

Create a Pod that generates some log output.
```shell
vi logging-pod.yml
```
```shell
apiVersion: v1
kind: Pod
metadata:
name: logging-pod
spec:
containers:
- name: busybox
image: busybox:stable
command: ['sh', '-c', 'while true; do echo "Writing to the log!"; sleep 5; done']
```
```shell
kubectl apply -f logging-pod.yml
```
Check the Pod's logs.
```shell
kubectl logs logging-pod
```
Specify the container to view logs for in case of multi-container pods using `-c` flag.
```shell
kubectl logs logging-pod -c busybox
```
