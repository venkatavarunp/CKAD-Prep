
## Kubernetes API

To get a list of pods that are in kubesystem namespace

```shell
kubectl get pods -n kube-system
```
To get a list of pods that are in kubesystem namespace using raw API form
```shell
kubectl get --raw /api/v1/namespaces/kube-system/pods
```
To get a specifc (Ex : etcd-k8s-control) pod that are in kubesystem namespace using raw API form
```shell
kubectl get --raw /api/v1/namespaces/kube-system/pods/etcd-k8s-control
```
