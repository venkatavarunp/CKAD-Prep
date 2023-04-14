# Services and Networking
## Controlling Network Access with NetworkPolices
### Kubernetes Networking
The Kubernetes cluster uses a virtual network which allows pods to communicate with other seamlessly, even if they are on different nodes.
### NetworkPolicy
A `NetworkPolicy` is a Kubernetes object that allows us to restrict network traffic to and from pods within the cluster network.

We can use `NetworkPolices` to block unnecessary or unexpected network traffic and make our applications more secure.
| Non-Isolated Pods | Isolated Pods | 
| :-------- | :------- | 
| Any Pod that is not selected by any `NetworkPolices` | Any Pod that is selected by at least 1 `NetworkPolicy` | 
| Open to all incoming and outgoing network traffic (`NetworkPolices` don't apply) | Only open to traffic that is explicitly allowed by a `NetworkPolicy` that selects the Pod | 
