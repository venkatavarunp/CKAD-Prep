# Implementing Health Checks in a Kubernetes Application
## Introduction
Kubernetes health checks provide a deeper level of control over how Kubernetes detects the state of your application containers. In this lab, you will work closely with health checks using Kubernetes probes to fine-tune how Kubernetes manages containers.

Solution
Log in to the server using the credentials provided:
```shell
ssh cloud_user@<PUBLIC_IP_ADDRESS>
```
Add a Liveness Probe
Edit the Deployment:
```shell
kubectl edit deployment comb-dashboard -n hive
```
Implement the liveness probe. Scroll down to your container, and add the following to the container spec (starting under terminationMessagePolicy):

Note: When copying and pasting code into Vim from the lab guide, first enter :set paste (and then i to enter insert mode) to avoid adding unnecessary spaces and hashes. To save and quit the file, press Escape followed by :wq. To exit the file without saving, press Escape followed by :q!.
```shell
```
        livenessProbe:
          httpGet:
            path: /
            port: 80
          initialDelaySeconds: 3
          periodSeconds: 3
```
```
Type ESC followed by :wq to save your changes.

Check the comb-dashboard Pod status. The Pod should be restarted automatically whenever errors arise:
```shell
kubectl get pods -n hive
```
If you wait a few moments and check the status again, you should see the restart count increase.

Add a Readiness Probe
Edit the Deployment:
```shell
kubectl edit deployment comb-monitor -n hive
```
Implement the readiness probe. Scroll down to your container, and add the following to the container spec (starting under terminationMessagePolicy):
```shell
        readinessProbe:
          exec:
            command: ['echo', 'This is a test!']
          initialDelaySeconds: 3
          periodSeconds: 3
```
Type ESC followed by :wq to save your changes.

Check the comb-monitor Pod status. The Pod should start up successfully and the container should become ready:
```shell
kubectl get pods -n hive
```
