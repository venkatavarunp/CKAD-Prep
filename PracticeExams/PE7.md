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
Run a Job on a Schedule
Create a CronJob:
```shell
vi pwd-runner.yml
```
Add the following to create a CronJob named pwd-runner in the one Namespace that uses the busybox:stable image. The Job should run the pwd command once every minute. If the command takes longer than 10 seconds to execute, it should be automatically terminated:

Note: When copying and pasting code into Vim from the lab guide, first enter :set paste (and then i to enter insert mode) to avoid adding unnecessary spaces and hashes. To save and quit the file, press Escape followed by :wq. To exit the file without saving, press Escape followed by :q!.
```shell
apiVersion: batch/v1
kind: CronJob
metadata:
  name: pwd-runner
  namespace: one
spec:
  schedule: "* * * * *"
  jobTemplate:
    spec:
      activeDeadlineSeconds: 10
      template:
        spec:
          restartPolicy: OnFailure
          containers:
          - name: busybox
            image: busybox:stable
            command: ['pwd']
```
Type ESC followed by :wq to save your changes.

Use kubectl apply to create the CronJob:
```shell
kubectl apply -f pwd-runner.yml
```
Check the CronJob's status:
```shell
kubectl get cronjob pwd-runner -n one
```
If you see <none> in the LAST SCHEDULE column, you may need wait a moment and rerun the command again to verify that it has successfully executed at least once. Once it runs, you will see a value in the LAST SCHEDULE column.

Run the verification script to ensure you completed the objective:
```shell
~/verify.sh
```
Create a Deployment
Create the Deployment:
```shell
vi nginx-dep.yml
  ```
Add the following to create a Deployment called nginx-dep in the two Namespace. Use the nginx:stable image. Set the replica count to 3. Expose port 8081, and add an environment variable to the containers called NGINX_PORT with the value 8081. This will cause the nginx container to listen on port 8081:
```shell
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-dep
  namespace: two
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx-dep
  template:
    metadata:
      labels:
        app: nginx-dep
    spec:
      containers:
      - name: nginx
        image: nginx:stable
        ports:
        - containerPort: 8081
        env:
        - name: NGINX_PORT
          value: "8081"
```
Type ESC followed by :wq to save your changes.

Create the Deployment with kubectl apply:
```shell
kubectl apply -f nginx-dep.yml
  ```
Check the Deployment's status:
```shell
kubectl get deployment nginx-dep -n two
  ```
You should see 3/3 Pods are in the READY status.

Check the Pods:
```shell
kubectl get pods -n two
  ```
You should see your 3 replicas with a Running STATUS.

Run the verification script to ensure you completed the objective:
```shell
~/verify.sh
  ```
