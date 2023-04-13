# Using Custom Resources (CRD)
Log in to the control plane node.

Create a CustomResourceDefinition.
```shell
vi beehives.yml
```
```shell
apiVersion: apiextensions.k8s.io/v1
kind: CustomResourceDefinition
metadata:
name: beehives.acloud.guru
spec:
group: acloud.guru
names:
plural: beehives
singular: beehive
kind: BeeHive
shortNames:
- hive
scope: Namespaced
versions:
- name: v1
served: true
storage: true
schema:
openAPIV3Schema:
type: object
properties:
spec:
type: object
properties:
supers:
type: integer
bees:
type: integer
```
```shell
kubectl apply -f beehives.yml
```
List the current CRDs.
```shell
kubectl get crd
```
Create an object using your new CRD.
```shell
vi test-beehive.yml
```
```shell
apiVersion: acloud.guru/v1
kind: BeeHive
metadata:
name: test-beehive
spec:
supers: 3
bees: 60000
```
```shell
kubectl apply -f test-beehive.yml
```
Interact with your custom object using kubectl commands.
```shell
kubectl get beehives
```
```shell
kubectl get hive
```
```shell
kubectl describe beehive test-beehive
```
