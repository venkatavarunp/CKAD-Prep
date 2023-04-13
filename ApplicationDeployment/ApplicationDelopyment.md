# Deployment

A deployment is a kubernetes object that defines a desired state for a set of replica pods. kubernetes constantly works to maintain that desired state by creating, deleting and replacing those pods.

So deployment allows us to manage a desired state across multiple replica pods using a `pod template`

`pod template` is a shared configuration used by the deployment to create new replicas.

## Scaling
the conmcept of deployment replicas makes pretty easy to achieve Scaling with our applications in kubernetes.

Each deployment has a replicas field that determines the number of replicas in that deployment.

we can change `replicas` value in order to `scale up` and `scale down`. By changing `replicas` we can change number of `replica pods`.

To achieve it we need to change the `Deployment configuration` and cluster will react by adding, removing based on the inputs.

[Click here](https://github.com/venkatavarunp/CKAD-Prep/blob/main/ApplicationDeployment/Labs/Scaling.md) to know how scaling works in kubernetes.

## Performing Rolling Updates

A Rolling update allows to change a deployment's pod template, gradually replaceing replicas with zero downtime.

kubernetes gets to the desired state by updating the replicas to reflect that in new pod template

kubernetes doesn't delete all replicas at once instead it gradually spins up new replicas and gradually remove the old replicas as the new ones become available.

[Click here](https://github.com/venkatavarunp/CKAD-Prep/blob/main/ApplicationDeployment/Labs/Scaling.md) to know how to roll updates in kubernetes.

## Deploying with Blue/Green and Canary Stratergies

### Deployment Stratergy 
A deployment stratergy is a method of rolling out new code that is used to achieve some benefit, such as increasing reliability and minimizing risk.

There are two deployment stratergies 

### 1. Blue/Green Deployment
A Blue/Green deployment stratergy involves using 2 identical production environments, usually called blue and green.

when we want to rollout new code, the new code is rolled out to the second environment. This environment is confirmed to be stable and working before user traffic is directed to the new environment.

### 2. Canary Deployment
Just like blue/green, a Canary deployment stratergy uses 2 environment.

Unlike Blue/Green a `portion` of the user base is directed to the new code in order to expose any issues before the changes are rolled out to everyone else.

We use small portion of user base to test to the new environment and if everything is fine we direct all of the users to updataed new production environment . It acts as a `beta` version.

- Using`labels` and `selectors` on services helps to direct user traffic to different Pods.

[Click here](https://github.com/venkatavarunp/CKAD-Prep/blob/main/ApplicationDeployment/Labs/DeploymentStratergies.md) to see how Deployment stratergies work. 

## Helm
Helm is a pcakage management tool for applications that runs in kubernetes. It allows you to easily install software in your cluster, alongside the neccessary kubernetes configuration.

When we setup an application, we create `YAML` files for deployments and services, and a lot of different kubernetes objects.

`Helm` allows us to take all that configuration put it in 1 pcakage and install applications in clusters that consists multiple different kubernetes objects with a single command.

## Helm Charts
A helm chart is a helm software package. it contains all of the kubernetes resource definitions needed to get the application up and running in the cluster.

The resource definition are the same thing as the `YAML` files inorder to create different objects like Pods, deployments, and services.

In order to install helm chart we need to interact with `helm repository`

## Helm repository
A helm repository is a collection of available charts. we can use it to browse and download charts before installing them in the cluster.

[Click here](https://github.com/venkatavarunp/CKAD-Prep/blob/main/ApplicationDeployment/Labs/Helm.md) to see how helm works using charts and repository.
