# Controlling Network Access with NetworkPolicies - Part 2
Log in to the control plane node.

Now let's create a NetworkPolicy to allow traffic coming from the client Pod.
```shell
vi np-test-client-allow.yml
```
```shell
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
name: np-test-client-allow
namespace: np-test-a
spec:
podSelector:
matchLabels:
app: np-test-server
policyTypes:
- Ingress
ingress:
- from:
- namespaceSelector:
matchLabels:
team: bteam
podSelector:
matchLabels:
app: np-test-client
ports:
- protocol: TCP
port: 80
```
```shell
kubectl apply -f np-test-client-allow.yml
```
Check the client Pod's log again. The traffic should be allowed now.
```shell
kubectl logs client-pod -n np-test-b
```
Let's create a setup to restrict outgoing traffic for the client Pod.

Create a default deny Egress policy for the np-test-b Namespace.
```shell
vi np-test-b-default-deny-egress.yml
```
```shell
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
name: np-test-b-default-deny-egress
namespace: np-test-b
spec:
podSelector: {}
policyTypes:
- Egress
```
```shell
kubectl apply -f np-test-b-default-deny-egress.yml
```
Check the client Pod's log. Traffic should now be denied.
```shell
kubectl logs client-pod -n np-test-b
```
Create a Policy to allow the Egress traffic from the client Pod. This time, we'll allow traffic to any Pod in the np-test-a
Namespace.
```shell
vi np-test-client-allow-egress.yml
```
```shell
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
name: np-test-client-allow-egress
namespace: np-test-b
spec:
podSelector:
matchLabels:
app: np-test-client
policyTypes:
- Egress
egress:
- to:
- namespaceSelector:
matchLabels:
team: ateam
ports:
- protocol: TCP
port: 80
```
```shell
kubectl apply -f np-test-client-allow-egress.yml
```
Check the client Pod's log. Traffic should be allowed once more.
```shell
kubectl logs client-pod -n np-test-b
```
