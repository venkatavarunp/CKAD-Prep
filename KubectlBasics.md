
## Kubectl

To get list of objects (Ex:Object-service)
services help to build microservices within kubernetes cluster.
```shell
kubectl get service
```
Creating a sample service file and adding some YAML configurations to it
```shell
vi my-service.yml
```
```shell
apiVersion: v1
kind: Service
metadata:
  name: my-service
spec:
  selector:
    app: my-app
ports:
  - protocol: TCP
    port: 80
    targetPort: 80
```
creating the service in cluster
```shell
kubectl create -f my-service.yml --save-config
```
To get detailed information particular object
```shell
kubectl describe service my-service 
```
To add modifications done to service and to apply them to reflect in the cluster
```shell
kubectl apply -f my-service.yml
```
To delete object from the cluster
```shell
kubectl delete service my-service
