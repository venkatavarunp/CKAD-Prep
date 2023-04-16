# Configuring Applications in Kubernetes
## Introduction
Kubernetes offers tools to help you manage application configuration within your cluster. This lab will give you the opportunity to get hands-on with the process of configuring your Kubernetes apps.

## Solution
Log in to the server using the credentials provided:
```shell
ssh cloud_user@<PUBLIC_IP_ADDRESS>
```
Create a ConfigMap
Create the YAML file for your ConfigMap:
```shell
vi honey-config.yml
```
Add the content for your ConfigMap:

Note: When copying and pasting code into Vim from the lab guide, first enter :set paste (and then i to enter insert mode) to avoid adding unnecessary spaces and hashes. To save and quit the file, press Escape followed by :wq. To exit the file without saving, press Escape followed by :q!.
```shell
apiVersion: v1
kind: ConfigMap
metadata:
  name: honey-config
  namespace: hive
data:
  honey.cfg: |
    There is always money in the honey stand!
```
Type ESC followed by :wq to save your changes.

Create the ConfigMap with kubectl apply:
```shell
kubectl apply -f honey-config.yml
```
Create a Secret
Get a base64-encoded string for the sensitive data:
```shell
echo secretToken! | base64
```
Copy the full string as it will be used next.

Create a YAML file for the Secret:
```shell
vi hive-sec.yml
```
Create the manifest and use the base64-encoded string you copied before for the hiveToken value:
```shell
apiVersion: v1
kind: Secret
metadata:
  name: hive-sec
  namespace: hive
type: Opaque
data:
  hiveToken: <BASE-64-ENCODED-STRING>
```
Type ESC followed by :wq to save your changes.

Create the Secret with kubectl apply:
```shell
kubectl apply -f hive-sec.yml
```
Modify the Deployment To Use the ConfigMap and Secret Data
Edit the Deployment:
```shell
kubectl edit deployment hive-mgr -n hive
```
Supply the Secret data to the container using an environment variable. Scroll down to the bottom of the container spec, and add the following to the bottom of the spec (below the terminationMessagePolicy line):
```shell
        env:
        - name: TOKEN
          valueFrom:
            secretKeyRef:
              name: hive-sec
              key: hiveToken
```
On the next line, provide the ConfigMap data as a mounted volume:
```shell
        volumeMounts:
        - name: config
          mountPath: /config
          readOnly: true
      volumes:
      - name: config
        configMap:
          name: honey-config
          items:
          - key: honey.cfg
            path: honey.cfg
```
Type ESC followed by :wq to save your changes.

Locate the Deployment's Pods:
```shell
kubectl get pods -n hive
```
You should see the old Pods have a status of Terminating, and the new ones are now Running.

Copy the NAME for one of the Running Pods, and use it to check the Pod log:
```shell
kubectl logs -n hive <Pod NAME>
```
The logs should display the data from both the ConfigMap and the Secret. It should look something like:

daily message: There is always money in the honey stand!
Authenticating with hiveToken secretToken!
