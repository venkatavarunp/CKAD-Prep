
# Building a Kubernetes 1.24 Cluster with kubeadm

## 1.Log in to the lab servers using the credentials provided:

```bash
ssh cloud_user@<PUBLIC_IP_ADDRESS>

```
## 2.Install Packages
Create configuration file for containerd:
```shell
cat <<EOF | sudo tee /etc/modules-load.d/containerd.conf
overlay
br_netfilter
EOF
```
Load modules:
```shell
sudo modprobe overlay
sudo modprobe br_netfilter
```
Set system configurations for Kubernetes networking:
```shell
cat <<EOF | sudo tee /etc/sysctl.d/99-kubernetes-cri.conf
net.bridge.bridge-nf-call-iptables = 1
net.ipv4.ip_forward = 1
net.bridge.bridge-nf-call-ip6tables = 1
EOF
```
Apply new settings:
```shell
sudo sysctl --system
```
Install containerd:
```shell
sudo apt-get update && sudo apt-get install -y containerd.io
```
Create default configuration file for containerd:
```shell
sudo mkdir -p /etc/containerd
```
Generate default containerd configuration and save to the newly created default file:
```shell
sudo containerd config default | sudo tee /etc/containerd/config.toml
```
Restart containerd to ensure new configuration file usage:
```shell
sudo systemctl restart containerd
```
Verify that containerd is running:
```shell
sudo systemctl status containerd
```
Disable swap:
```shell
sudo swapoff -a
```
Install dependency packages:
```shell
sudo apt-get update && sudo apt-get install -y apt-transport-https curl
```
Download and add GPG key:
```shell
curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -
```
Add Kubernetes to repository list:
```shell
cat <<EOF | sudo tee /etc/apt/sources.list.d/kubernetes.list
deb https://apt.kubernetes.io/ kubernetes-xenial main
EOF
```
Update package listings:
```shell
sudo apt-get update
```
Install Kubernetes packages (Note: If you get a dpkg lock message, just wait a minute or two before trying the command again):
```shell
sudo apt-get install -y kubelet=1.24.0-00 kubeadm=1.24.0-00 kubectl=1.24.0-00
```
Turn off automatic updates:
```shell
sudo apt-mark hold kubelet kubeadm kubectl
```
Log in to both worker nodes to perform previous steps.

## 3.Initialize the Cluster
On the control plane node, initialize the Kubernetes cluster on the control plane node using kubeadm:
```shell
sudo kubeadm init --pod-network-cidr 192.168.0.0/16 --kubernetes-version 1.24.0
```
Set kubectl access:
```shell
mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config
```
Test access to cluster:
```shell
kubectl get nodes
```
## 4.Install the Calico Network Add-On
On the control plane node, install Calico Networking:
```shell
kubectl apply -f https://raw.githubusercontent.com/projectcalico/calico/v3.25.0/manifests/calico.yaml
```
Check status of the control plane node:
```shell
kubectl get nodes
```
## 5.Join the Worker Nodes to the Cluster
In the control plane node, create the token and copy the kubeadm join command:
```shell
kubeadm token create --print-join-command
```
Note: This output will be used as the next command for the worker nodes.

Copy the full output from the previous command used in the control plane node. This command starts with 
```shell
kubeadm join.
```
In both worker nodes, paste the full kubeadm join command to join the cluster. Use sudo to run it as root:
```shell
sudo kubeadm join... 
```
In the control plane node, view cluster status:
```shell
kubectl get nodes
```
Note: You may have to wait a few moments to allow all nodes to become ready.
