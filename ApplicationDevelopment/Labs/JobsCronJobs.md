# Running Jobs and CronJobs
Log in to the control plane node.

Create a simple Job that runs the command echo This is a test! .
```shell
vi my-job.yml
```
```shell
apiVersion: batch/v1
kind: Job
metadata:
  name: my-job
spec:
  template:
    spec:
      containers:
      - name: print
        image: busybox:stable
        command: ["echo", "This is a test!"]
      restartPolicy: Never
  backoffLimit: 4
  activeDeadlineSeconds: 10
```
Jobs are part of `batch/v1` API

A job contains a pod template specifications so `template` indicates Pod template

So Jobs actually run our containerized code using pods
Run the Job.

For a Job or CronJob `restartPolicy` must be `OnFailure` or `Never` as Jobs runs only once so they shouldn't be restarted.

`activeDeadlineSeconds`  in Job spec terminates the job if it runs too long
```shell
kubectl apply -f my-job.yml
```
Check the status of the Job.
```shell
kubectl get jobs
```
View the Job output. First, you will need to find the name of the Job's Pod.
```shell
kubectl get pods
```
```shell
kubectl logs $JOB_POD_NAME
```
Create a CronJob that will run the Job task every minute.
```shell
vi my-cronjob.yml
```
```shell
apiVersion: batch/v1
kind: CronJob
metadata:
  name: my-cronjob
spec:
  schedule: "*/1 * * * *"
  jobTemplate:
    spec:
      template:
        spec:
          containers:
          - name: print
            image: busybox:stable
            command: ["echo", "This is a test!"]
          restartPolicy: Never
      backoffLimit: 4
      activeDeadlineSeconds: 10
```
`jobTemplate` is the template of Pod or Job that we need to run periodically

`schedule`  sets schedule to regularly excute the job.`*/1 * * * *` is a Linux Cron Expression.
```shell
kubectl apply -f my-cronjob.yml
```
Check the CronJob status.
```shell
kubectl get cronjob
```
