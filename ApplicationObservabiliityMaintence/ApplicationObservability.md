# Application Observability and Observability
## API Deprecation Policy

The `kubernetes API` is the primary interface of kubernetes.
when we run command we communiate with API which is a part of control plane and `API` returns the information

`kubectl` is actually just a tool that allows to communicate with `Kubernetes API`.

### Deprecation 
Deprecation is the process of providing advanced warning of changes to an API, giving consumers of that API time to update their code and/or processes.

So as new versions of kubernetes come out, occasionally there are changes to those `Kubernetes API's` and Kubernetes provides advanced warning of those changes so that our tools and other things that are interacting with the API aren't broken without us having some kind of chance. To prepare for that change.

### Highlights of Deprecation Policy
### `apiVersion`
the `apiVersion` field on kubernetes object indicates which version of the API that `YAML` is designed to be compatible with.
### Deprecation Window
Deprecation Window gives an idea on how much time you have once those changes are announced to adapt your tools that might be interacting with the `Kubernetes API` and that Deprecation Window is different for features that are in alpha, feature that are in beta, and feature that are in GA (general availablity)

`GA` features are considered to be fully tested and fully released and these have longest Deprecation window and that is 12 months or three releases(kubernetes versions).
### Migration Guide
A page in kubernetes documentation that gives detailed information about API changes, Deprecation windows for API's.

#### `API Deprecation` is the process of announcing changes to an API early, giving users time to update their code and/or tools.
#### Kubernetes removes support for Deprecated APIs that aer in GA (general availablity) only after 12 months or 3 Kubernetes releases , whichever is longer.
## Probes and Health Checks
### Probe
Probes are part of container soec in kubernetes. they allow us to customize how kubernetes detects the state of a container.

They also allow us to implement different kinds of health checks for our containers.

There are three kinds of probes.
1. `Liveness`
Liveness probes check whether a container is healthy so that it can be restarted if it becomes unhealthy.

2. `Readiness`
Readiness probes determine when a container is fully started up and ready to receive user traffic.

3. `Startup`
Startup probes are a special case designed for slow starting containers that take a long time to start running.
These are `Liveness` that are only active during that slow startup time.
the purpose of `startup` is to prevent the container being considered unhealthy during that long startup times.

[Click here] (https://github.com/venkatavarunp/CKAD-Prep/new/main)
