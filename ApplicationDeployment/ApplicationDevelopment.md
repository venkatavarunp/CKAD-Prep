# Deployment

A deployment is a kubernetes object that defines a desired state for a set of replica pods. kubernetes constantly works to maintain that desired state by creating, deleting and replacing those pods.

So deployment allows us to manage a desired state across multiple replica pods using a `pod template`

`pod template` is a shared configuration used by the deployment to create new replicas.

## Scaling
the conmcept of deployment replicas makes pretty easy to achieve Scaling with our applications in kubernetes.

Each deployment has a replicas field that determines the number of replicas in that deployment.

we can change `replicas` value in order to `scale up` and `scale down`. By changing `replicas` we can change number of `replica pods`.

To achieve it we need to change the `Deployment configuration` and cluster will react by adding, removing based on the inputs.
