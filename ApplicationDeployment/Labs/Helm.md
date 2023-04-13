# Helm

Log in to the control plane node.

Setup the Helm GPG key and package repository.
```shell
curl https://baltocdn.com/helm/signing.asc | sudo apt-key add -
```
```shell
echo "deb https://baltocdn.com/helm/stable/debian/ all main" | sudo tee /etc/apt/sources.list.d/helmstable-debian.list
```
Install the Helm package.
```shell
sudo apt-get update
```
```shell
sudo apt-get install -y helm
```
Verify the installation.
```shell
helm version
```
## Using Helm Charts and Repository

Add a Helm chart repository.
```shell
helm repo add bitnami https://charts.bitnami.com/bitnami
```
Update the repository.
```shell
helm repo update
```
View a list of charts available in the repository.
```shell
helm search repo bitnami
```
Create a Namespace.
```shell
kubectl create namespace dokuwiki
```
Install a chart.
```shell
helm install --set persistence.enabled=false -n dokuwiki dokuwiki bitnami/dokuwiki
```
View some of the objects created by the Helm install.
```shell
kubectl get pods -n dokuwiki
kubectl get deployments -n dokuwiki
kubectl get svc -n dokuwiki
```
Uninstall the release and delete the Namespace to clean up.
```shell
helm uninstall -n dokuwiki dokuwiki
```
```shell
kubectl delete namespace dokuwiki
```
