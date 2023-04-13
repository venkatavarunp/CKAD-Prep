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

### Authentication and Authorization
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

### Role-Based Access Control (RBAC)
RBAC is a system for managing authorization. It allows to define what users, or groups of users, are allowed to do within the kubernetes cluster.

- Normal users authenticate using `client certificates`, ServiceAccounts uses `tokens`.
- Authorization for both normal users and ServiceAccount can be managed using RBAC
- Roles and ClusterRoles define a specific set of permissions.
- RoleBindings and ClusterRoleBindings tie Roles por ClusterRoles to users/ServiceAccounts

[Click here](https://github.com/venkatavarunp/CKAD-Prep/blob/main/ApplicationEnvironment%2CConfigurationSecurity/Labs/KubernetesAuth.md) to know more about kubernetes authorization.
