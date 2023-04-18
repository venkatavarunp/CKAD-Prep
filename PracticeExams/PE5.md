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
Create a PersistentVolume
Create the PersistentVolume:
```shell
vi pv-data.yml
```
Add the following to create a PersistentVolume called pv-data in the default Namespace. Ensure you set the storage to 1Gi and the storageClassName to static. Use a hostPath to mount the host directory /var/pvdata with the PersistentVolume:

Note: When copying and pasting code into Vim from the lab guide, first enter :set paste (and then i to enter insert mode) to avoid adding unnecessary spaces and hashes. To save and quit the file, press Escape followed by :wq. To exit the file without saving, press Escape followed by :q!.
```shell
apiVersion: v1
kind: PersistentVolume
metadata:
  name: pv-data
spec:
  capacity:
    storage: 1Gi
  accessModes:
    - ReadWriteOnce
  storageClassName: static
  hostPath:
    path: /var/pvdata
```
Type ESC followed by :wq to save your changes.

Apply the changes with kubectl apply:
```shell
kubectl apply -f pv-data.yml
```
Check the PersistentVolume status:
```shell
kubectl get pv pv-data
```
You should see the information for the PersistentVolume with a STATUS of Available.

Run the verification script to ensure you completed the objective:
```shell
~/verify.sh
```
Create a Pod That Consumes Storage from the PersistentVolume
Create the PersistentVolumeClaim:
```shell
vi pv-data-pvc.yml
```
Add the following to create the PersistentVolumeClaim called pv-data-pvc. Configure the specification so that this PersistentVolumeClaim will bind to the PersistentVolume created earlier:
```shell
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: pv-data-pvc
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 256Mi
  storageClassName: static
```
Type ESC followed by :wq to save your changes.

Apply the changes with kubectl apply:
```shell
kubectl apply -f pv-data-pvc.yml
```
Check the PersistentVolumeClaim status:
```shell
kubectl get pvc pv-data-pvc
```
A Bound status means that the PersistentVolumeClaim has successfully bound itself to the pv-data volume.

Create the Pod:
```shell
vi pv-pod.yml
```
Add the following to create a Pod named pv-pod. Use the busybox:stable image and below command. Use the PersistentVolume to provide storage to this Pod's container at the path /var/data:
```shell
apiVersion: v1
kind: Pod
metadata:
  name: pv-pod
spec:
  containers:
  - name: busybox
    image: busybox:stable
    command: ['sh', '-c', 'while true; do sleep 3600; done']
    volumeMounts:
    - name: static-content
      mountPath: /var/data
  volumes:
  - name: static-content
    persistentVolumeClaim:
      claimName: pv-data-pvc
```
Type ESC followed by :wq to save your changes.

Apply the changes with kubectl apply:
```shell
kubectl apply -f pv-pod.yml
```
Check the Pod status:
```shell
kubectl get pod pv-pod
```
You should see the Pod is in the Running STATUS.

Use kubectl exec to verify that the PersistentVolume's hostPath data is accessible from within the container:
```shell
kubectl exec pv-pod -- ls /var/data
```
You should see data.txt returned in the output. This is static content that is already on the Kubernetes worker node, and it is being pulled in via the PersistentVolume.

Review the contents of the data.txt file:
```shell
kubectl exec pv-pod -- cat /var/data/data.txt
```
You should see a string returned indicating you can successfully access the PersistentVolume's storage data through the Pod's container.

Run the verification script to ensure you completed the objective:
```shell
~/verify.sh
```
