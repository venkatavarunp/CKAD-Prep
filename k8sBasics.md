# Documentation allowed for CKAD exam
- [Kubernetes Docs](https://kubernetes.io/docs)
- [Kubernetes GitHub](https://github.com/kubernetes/)
- [Kubernetes Blog](https://kubernetes.io/blog/)
- [Helm Docs](https://helm.sh/docs/)
# Basics Of Kubernetes

## Containers
Container is a standard unit of software. The packages up coat and all of its dependencies for the application runs quickly and reliably from one computing environment to another.
## Advantages of Containers
- Portability (Containers lets software to run in a variety of environments)
- Consistency (Containers helps software to run in same way irrespective of environment)
- Low Overhead (Containers uses fewer resources than other technologies like VM's)
## Container Runtimes
It is a software used to run Containers on a machine
# Kubernetes
Kubernetes also known as K8s, is an Open source system for automating deployment, scaling and management of containerised applications.
Kubernetes can also helps Containers in 
- Networking (handles networking, so provide any networking framework that helps manage and control network communication between Containers)
- Security (provides a variety of different security features that give you the ability to build more secure applications within your Kubernetes infrastructure)
- Configuration Management (features that helps to manage application configuration and pass configuration data to the containers)
# Kubernetes Cluster
A Kubernetes Cluster is a collection of worker machines that runs Containers.
## Control Plane
A collection of services that control the Cluster
- Users interact with the clusters using the control Plane
- Monitors the state of the Cluster 
- Control plane has multiple individual components and these can run anywhere and can be run in multiple instances for High availability.
In reality Kubernetes control plane is the collection of services. These are essentially just applications that really could be running anywhere. (Usually they're all running on a server,we refer to that server as the controlling server). In reality it is just a collection of multiple different pieces of software. And these services control the cluster. Users interact with the Kubernetes cluster using the control plane and control plane also monitors the state of the cluster.
## Worker Nodes
Worler nodes require a Container runtime to manage containers, and use a component called `Kubelet` to manage Kubernetes activity on the node.
## Kubelet
It is a Kubernetes agent that runs on each node and manages Kubernetes activity on that node
## Kubeadm
Kubeadm is a tool that will simplify the process of setting up our Kubernetes cluster.

[Click here](https://github.com/venkatavarunp/CKAD-Prep/blob/main/KubernetesCluster.md) for Building a Kubernetes 1.24 Cluster with kubeadm.


## The Kubernetes API
The core of Kubernetes control plane is the API server
- HTTP API (It is a basic HTTP API)
- User Interface (The API lets users query and manipulate objects, thereby controlling the cluster)
- Central point of communication ( Various components of Kubernetes communicate with each other using the API)
The API acts as the Interface between users and clusters.

[Click here](https://github.com/venkatavarunp/CKAD-Prep/blob/main/KubernetesCluster.md) for more information about for Kubernetes API
## Kubernetes Objects
Kubernetes Objects are persistent dat entites stored by Kubernetes.They represent the state of the cluster
We can deploy and configure applications, run containers, and configure cluster behavior by creating, modifying, and deleting objects and all this happens through `Kubernetes API`.
## Pods 
Most important kubernetes object that is used to run and manage containers.
## YAML 
Kubernetes objects are represented in form of YAML data.It is a data format that allows us to represent the data that is part of kubernetes objects.
## API Version
It is a YAML document that specifies which version of the kubernetes API the YAML data is compatable with

Sample YAML code 
```YAML 
apiVersion: V1
kind: Pod
metadata:
    name: nginx-pod 
spec:
    containers:
        name: nginx
        image: ngnix
```
## Kubectl 
It is a command line tool which allows you to run commands against kubernetes clusters. We can use kubectl to deploy of applications, inspect and manage cluster resources and view logs.

In simple it helps us to interact with kubernetes from command line.
It works primarly by allowing you to view, create, modify, and delete kubernetes objects.

kubectl communicates with Kubernetes API to carry out commands 

[Click here](https://github.com/venkatavarunp/CKAD-Prep/blob/main/KubectlBasics.md) for viewing basic kubectl commands

## Pod
A pod is a group of one or more containers with shared storage and network resources, and a specification for how to run containers

In other words, A pod is a kubernetes object that allows us to run containers in cluster. A pod can run more than one container, But it has to run atleast one container and if it runs more than one container these containers do need to be closely connected and actually share some resources 

It is always preferred to run two containersin seperate pods unless there is an exclusive case to run two containers in same pod.
## Pod specification Basics
Sample YAML file
```shell
apiVersion: V1
kind: Pod
metadata:
  name: ngnix-pod 
spec:
- name: nginx
  image: ngnix
```
`kind` - specifies the object type as Pod.

`metadata.name` - the name of the Pod object

`spec.containers` - provides a list of one or more containers included in Pod.

`spec.container[].image` - specifiesthe name of a container image containing the software you want to run.

[Click here](https://github.com/venkatavarunp/CKAD-Prep/blob/main/PodContainers.md) for viewing Managing Containers with Pods.
# Kubernetes in CLoud
Many cloud providers offer `Managed Kubernetes `Platforms, allowing to spin up a cluster with a few clicks
## Kubernetes on AWS 
` Amazon EKS(Elastic Kubernetes Service) ` allows to run applications on a managed kubernetes infrastructure and it's an on demand service

`Amazon EC2` are just VMs that run in AWS but we can use EC2 machines to install Kubernetes and setup own kubernetes cluster.These helps to customize kubernetes than AWS EKS.

`Amazon ECR(Elastic Container Registry) `It is a place to safely securely store and manage container images that run in kubernetes

## Azure Kubernetes Service (AKS)
`AKS` provides a managed kubernetes Platform within Microsoft Azure.

`AKS` can integrate with Azure's continous integration/continous delivery (CI/CD) features.

## Google Kubernetes Engine (GKE)
 `GKE` includes some useful feature for high availability, such as multi-zone and regional clusters.
