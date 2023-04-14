# Application Environment, Configuration and Security
## Custom Resources (CRD)
A Custom resource is an  extension of the kubernetes API. It allows you to define your own custom Kubernetes object types and interact with them, just as we would with other kubernetes objects.

Custom Resources allows us to create own kubernetes objects 

`CustomResourceDefinitions` (CRD) defines a custom resource (customised kubernetes objects). 

[Click here](https://github.com/venkatavarunp/CKAD-Prep/blob/main/ApplicationEnvironment%2CConfigurationSecurity/Labs/CRD.md) to know more about CRD in kubernetes.
## ServiceAccounts
A ServiceAccount allows processes within containers to authenticate with the kubernetes API server. They can be assigned permissions via role-based access control, just like regular user accounts.

if we need to interact with the kubernetes API from inside of kubernetes containers, we use `ServiceAccount` to authenticate and manage permissions for that process.

### ServiceAccount Tokens
A `token` is the crendential used to access the API using `ServiceAccount`.

The token for a Pod's ServiceAccountis **automatically mounted** to each container by Kubernetes

## Authentication and Authorization
when user request to the API the the API **authenticates(is the user who they claim to be)** with control plane and then the control plane **Authorizes(Does the user have permission to do this?)** the user when they can access or not.

In kubernetes ther are two different types of uses that might try to authenticate. 
| Regular Users | ServiceAccounts | 
| :-------- | :------- | 
| Not represented by a kubernetes object | represented by kubernetes object | 
| Cannot be created in kubernetes | Can be created in kubernetes by `YAML` files | 
| Authenticates using a client certificate | Authenticates using API token |
| Used by human users or tools running outside the cluster | Used by automated processes running within containers |

Despite the differences **both `users` and `ServiceAccounts` receive Authorization using Role Based access control (RBAC)**

We can use RBAC to control permissions for both regular `users` and `ServiceAccount` in the same way.

## Role-Based Access Control (RBAC)
RBAC is a system for managing authorization. It allows to define what users, or groups of users, are allowed to do within the kubernetes cluster.

- Normal users authenticate using `client certificates`, ServiceAccounts uses `tokens`.
- Authorization for both normal users and ServiceAccount can be managed using RBAC
- Roles and ClusterRoles define a specific set of permissions.
- RoleBindings and ClusterRoleBindings tie Roles por ClusterRoles to users/ServiceAccounts

[Click here](https://github.com/venkatavarunp/CKAD-Prep/blob/main/ApplicationEnvironment%2CConfigurationSecurity/Labs/KubernetesAuth.md) to know more about kubernetes authorization.

## Admission Controller
Admission controllers intercepts requests to the kubernetes API after Authentication and authorization, but before any objects are persisted. They can be used to validate, deny, or even modify the request.

We can enable Admission controllers using `--enable-admission-plugins` flag for kube-apiserver

[Click here](https://github.com/venkatavarunp/CKAD-Prep/blob/main/ApplicationEnvironment%2CConfigurationSecurity/Labs/AdmissionControl.md) to see an example of admission control.

## Managing Compute Resource Usage
| Requests | Limits | 
| :-------- | :------- | 
| Provides Kubernetes with an idea of how many resources a container is expected to use | Provides an upper limit on how many resources a container is allowed to use.
| The cluster uses this information to select a node that has enough resources available | The cluster uses this information to terminate the container process if it attemps to use more than the allowed amount. |

### ResourceQuota
A `ResourceQuota` is a kubernetes object that sets limits on the resources used within a namespace if creating or modifying a resource would go beyond this limit, the request will be terminated.

[Click here](https://github.com/venkatavarunp/CKAD-Prep/blob/main/ApplicationEnvironment%2CConfigurationSecurity/Labs/ManagingCompute.md) to see an example of Maning Compute resource usage.

## Configuring Applications with ConfigMaps and Secrets 
### ConfigMap 
A ConfigMap is an object that stores configuration data for applications in the form of key-value pairs. This data can be passed to containers at runtime.
### Secret 
A Secretstores data that can be passed to container , but it is designed to store sensitive data such as password or API tokens.

There are two ways to pass `ConfigMap` and `Secret` data to containers.
| Volume Mounts | Environment Variables | 
| :-------- | :------- | 
| Data appears in the container's file system at runtime | Data appears as environment variables visible to the container at runtime.
| Each top-level key in the config data becomes file name | We can specific keys and variable names for each piece of data. |

[Click here](https://github.com/venkatavarunp/CKAD-Prep/blob/main/ApplicationEnvironment%2CConfigurationSecurity/Labs/ConfigMaps.md) to see an example of ConfigMaps and Secrets in Kubernetes.

## Configuring SecurityContext for Containers 
### Security Context 
A security context is part of the pod and container spec. it allows us to control advanced security-related settings for containers.

Here are few things `security context` can do
1. **`User ID` and `Group ID`**
We can customize the user ID (UID) and/or group ID (GID) used by the container process.

Example : If we want to use a user that has fewer permissions than what would otherwise be used, we can use `SecurityContext` to customize that.

2. **Allow Privilege Escalation**
We can enable or disable privilege escalation for the container process.

Privilege escalation allows a process to request more permissions than parent process. That's something we want to avoid as per security perspective.We can enable or disable on a container-by-container basis using SecurityContext settings.

3. **`Read-Only Root FileSystem`**
We can set the container's root filesystem to read-only to avoid someone to gain control or edit container process.

[Click here](https://github.com/venkatavarunp/CKAD-Prep/blob/main/ApplicationEnvironment%2CConfigurationSecurity/Labs/SecurityContext.md) to see an example of SecurityContext in Kubernetes.
