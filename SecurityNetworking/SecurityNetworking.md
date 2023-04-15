# Services and Networking
## Controlling Network Access with NetworkPolices
### Kubernetes Networking
The Kubernetes cluster uses a virtual network which allows pods to communicate with other seamlessly, even if they are on different nodes.

Kubernetes consists `Virtual Cluster NEtwork` that spans across all the nodes.
### NetworkPolicy
A `NetworkPolicy` is a Kubernetes object that allows us to restrict network traffic to and from pods within the cluster network.

We can use `NetworkPolices` to block unnecessary or unexpected network traffic and make our applications more secure.
| Non-Isolated Pods | Isolated Pods | 
| :-------- | :------- | 
| Any Pod that is not selected by any `NetworkPolices` | Any Pod that is selected by at least 1 `NetworkPolicy` | 
| Open to all incoming and outgoing network traffic (`NetworkPolices` don't apply) | Only open to traffic that is explicitly allowed by a `NetworkPolicy` that selects the Pod | 

**If a Pod is selected by any `NetworkPolicy`, traffic will be blocked unless it is allowed by at leaast 1 `NetworkPolicy` that selects the Pod.**

**If we combine a `namespaceSelector` and a `podSelector` within the same rule, the traffic must meet both the Pod- and Namespace-related conditions on order to be allowed** 

[Click here](https://github.com/venkatavarunp/CKAD-Prep/blob/main/SecurityNetworking/Labs/NetworkAccessP1.md) to see how Network Access is working in Kubernetes.

Even if a `NetworkPolicy` allows outgoing traffic from the source pod, `NetworkPolices` could still block the same traffic when it is incoming to the destination Pod.

[Click here](https://github.com/venkatavarunp/CKAD-Prep/blob/main/SecurityNetworking/Labs/NetworkAccessP2.md) to see how Ingress and Egrress works in Kubernetes.

## Services
A `Service` allows us to expose an application running across multiple Pods to the network.

Clients can communicate with the service, and traffic will be automatically routed to one of the underlying Pods.

If we have an application that consists of multiple replica pods, we can build a service on top of all those Pods. and the clients that are reaching out to that service, can communicate with the service and automatically be routed to any of underlying Pods.

### ClusterIP
A `ClusterIP` service exposes the application within the cluster network where it can be accessed by other Pods.
### NodePort
A `NodePort` service exposes the application externally by listening on an external port on each cluster node.

[Click here](https://github.com/venkatavarunp/CKAD-Prep/blob/main/SecurityNetworking/Labs/Services.md) to see how Services work.

## Exposing Applications with Ingress
### Ingress
An `Ingress` is a Kubernetes object that manages access to Services from outside the cluster.

As an Example : we can use an ingress to provide ur `end users` with access to the web application running in Kubernetes.

### Ingress Routing
An `Ingress` routes traffic to a service, which then routes the traffic to a Pod.

Ingress acts as a middleman in between the client and a service.Client makes a request to Ingress and Ingress fowards the request to service and then to the Pods.

**We nedd an `Ingress Controller` to implement the `Ingress` functionality.They are enabled as default when we create clusters**

[Click here](https://github.com/venkatavarunp/CKAD-Prep/blob/main/SecurityNetworking/Labs/Ingress.md) to see how Ingress work.
