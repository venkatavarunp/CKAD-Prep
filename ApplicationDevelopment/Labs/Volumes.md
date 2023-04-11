
# Demo on `hostPath`, `emptyDir` volumes
## 1. hostPath
Create a directory and a file with some data on worker node 1.
```shell
sudo mkdir /etc/hostPath
```
```shell
echo "This is worker 1!" | sudo tee /etc/hostPath/data.txt
```
Log in to worker node 2.

Create a directory and a file with some data on worker node 2.
```shell
mkdir /etc/hostPath
```
```shell
echo "This is worker 2!" | sudo tee /etc/hostPath/data.txt
```
Log in to the control plane node.

Create a Pod that uses a hostPath volume to mount the data from the host.
```shell
vi hostpath-volume-test.yml
```
```shell
apiVersion: v1
kind: Pod
metadata:
    name: hostpath-volume-test
spec:
  restartPolicy: OnFailure
  containers:
  - name: busybox
    image: busybox:stable
    command: ['sh', '-c', 'cat /data/data.txt']
    volumeMounts:
    - name: host-data
      mountPath: /data
  volumes:
  - name: host-data
    hostPath:
      path: /etc/hostPath
      type: Directory
```
`volumes` field in pod spec defines details about volumes used in the pod.

`volumeMounts` field in container spec mounts a volume to a specific container at a specific location.

`hostPath` volumes mount data from a specific location on the host (k8s node).

`hostPath` volume types:
- `Directory` mounts an existing directory on the host
- `DirectoryOrCreate` mounts a directory on the host, and creates it if doesn't exist.
- `File` mounts an existing single file on the host.
```shell
kubectl apply -f hostpath-volume-test.yml
```
Check the Pod status.
```shell
kubectl get pod hostpath-volume-test
```
Check the Pod logs. You should see output from the host data indicating which worker node the Pod is running on.
```shell
kubectl logs hostpath-volume-test
```
## 2. emptyDir
Now let's create a Pod that uses an emptyDir volume.
```shell
vi emptydir-volume-test.yml
```
```shell
apiVersion: v1
kind: Pod
metadata:
  name: emptydir-volume-test
spec:
  restartPolicy: OnFailure
  containers:
  - name: busybox
    image: busybox:stable
    command: ['sh', '-c', 'echo "Writing to the empty dir..." > /data/data.txt; cat /data/data.txt']
    volumeMounts:
    - name: emptydir-vol
      mountPath: /data
  volumes:
- name: emptydir-vol
  emptyDir: {}
```
```shell
kubectl apply -f emptydir-volume-test.yml
```
Get the container logs.
```shell
kubectl logs emptydir-volume-test
```
