# Application Development in Kubernetes
## Building Container Images
An Container Image is a lightweight , standalone file that contains software and executables needed to run a container.

A container image helps us to package a application so that we can eun it using container technology.

Once you package your application into an `image` you can use it to run any number of `containers`. 

`Docker` is one of the tool that can be used to create own images.
- A `Dockerfile` defines what is contained in that image
- the `docker build` command builds an image using the `Dockerfile`.
[Click here](https://github.com/venkatavarunp/CKAD-Prep/blob/main/ContainerImageDev.md) to checkout steps to install Docker and create own image using Kubernetes.
## Running Jobs and CronJobs
### Kubernetes Job 
Kubernetes Jobs are essentially objects that are designed to run a containerized task succesfully to completion

A Job helps us to run single task to completion 

### CronJob  
CronJob runs Job periodically according to a schedule

- If you want to run a task once , you can use Job
- If you want to run a task every minute/hour/Day or according to any type of schedule , you can use a CronJobs
[Click here](https://github.com/venkatavarunp/CKAD-Prep/blob/main/JobsCronJobs.md) to checkout steps to create a Job and CronJob.

## Building Multi-Container Pods
Multi-Container Pods are pods that include multiple containers that work together.

There are several different design patterns that we can use to actually make use of multi-container pod feature to provide value in applications.

### 1.SideCar Design Pattern 
A SideCar is an additional Container that performs tasks to assist main container

Example Usage: The main container serves files from shared volume. The sidecar periodically updates those files.

### 2.Ambassador Design Pattern
An Ambassador is an additional container that proxied network traffic to and fro from main container.

Example Usage : The main container reaches out to a service on port 80, but the service port chanegs to 81.Instead of changing the code to reach out to port 81 we could instead use an ambassador container to proxy that traffic from port 80 to 81 and make that port a little bit more configurable.

### 3.Adapter Design Pattern
An adapter container transforms the main containers output in some way
Example : if the main container outputs some log data to the container log in a non standard format, The adapter transforms the data into standard format by adding timestamps.

### When Should we use Multi-Container Pods ?
Use multi-container pods only when the containers need to be tightly coupled, sharing resources such as network and storage volumes.
It is always prefered to run multiple containers in pods untill or unless they are needed to run in same pods as per above case.

[Click here](https://github.com/venkatavarunp/CKAD-Prep/blob/main/MultiContainerPods.md) to checkout steps to create a Multi-Container Pods.
