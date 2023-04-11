# PersistentVolume
In the `hostPath`, we created a file in the directory /etc/hostPath on both of our worker nodes.

Log in to the control plane node.

Create a PersistentVolume that mounts the data from the host.
```shell
vi hostpath-pv.yml
```
```shell
apiVersion: v1
kind: PersistentVolume
metadata:
name: hostpath-pv
spec:
capacity:
storage: 1Gi
accessModes:
- ReadWriteOnce
storageClassName: slow
hostPath:
path: /etc/hostPath
type: Directory
```
```shell
kubectl apply -f hostpath-pv.yml
```
Check the status of the PersistentVolume.
```shell
kubectl get pv
```
Create a PersistentVolumeClaim that will bind to the PersistentVolume.
```shell
vi hostpath-pvc.yml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
name: hostpath-pvc
spec:
accessModes:
- ReadWriteOnce
resources:
requests:
storage: 200Mi
storageClassName: slow
```
```shell
kubectl apply -f hostpath-pvc.yml
```
Check the status of the PersistentVolumeClaim to verify that is has bound itself to the PersistentVolume. The STATUS should be
Bound , and the VOLUME should show the name of the PersistentVolume.
```shell
kubectl get pvc hostpath-pvc
```
Create a Pod that mounts the PersistentVolumeClaim.
```shell
vi pv-pod-test.yml
```
```shell
apiVersion: v1
kind: Pod
metadata:
name: pv-pod-test
spec:
restartPolicy: OnFailure
containers:
- name: busybox
image: busybox:stable
command: ['sh', '-c', 'cat /data/data.txt']
volumeMounts:
- name: pv-host-data
mountPath: /data
volumes:
- name: pv-host-data
persistentVolumeClaim:
claimName: hostpath-pvc
```
```shell
kubectl apply -f pv-pod-test.yml
```
Check the Pod status.
```shell
kubectl get pod pv-pod-test
```
Check the Pod logs. You should see output from the PersistentVolume data indicating which worker node the Pod is running on.
```shell
kubectl logs pv-pod-test
```
