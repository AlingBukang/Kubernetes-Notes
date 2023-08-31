**Labels, Selectors, Annotations**
- standard methods to group objects together based on a criteria
- used to record other details such as build version, etc.

`kubectl get pods --selector <selector-name>=<value>`

Multiple selectors:
`kubectl get all --selector env=prod,bu=finance,tier=frontend`


**Rolling updates and Rollbacks**
- rolling update is the default deploy strategy

```yml
strategy:
    rollingUpdate:
      maxSurge: 25%
      maxUnavailable: 25%
    type: RollingUpdate
```

To rollout:
`kubectl rollout status deployment/<deployment-name>`

`kubectl rollout history deployment/<deployment-name>`

To rollback:
`kubectl rollout undo deployment/<deployment-name>`
Use `--to-revision` flag to rollback to a specific version

**Jobs**
Type of Workloads:
- web server
- application
- database

Temp jobs:
- batch processing
- reporting
- analytics

`restartPolicy` - value can be `Always` or `Never`

job-definition.yaml
```yml
apiVersion: batch/v1
kind: Job
metadata:
    name: <job-name>
spec:
    [completions: 3] # run multiple jobs/pods one after the other
    [parallelism: 3] # run 3 jobs/pods in parallel
    template:
        spec:
            containers:
                - name: <name>
                  image: <image-name>
                  command: ['command','list','here'] 
            restartPolicy: Never         
```
Create job:
`kubectl create -f job-definition.yaml`
Display jobs:
`kubectl get jobs`
Check job logs:
`kubectl logs <job-name>`
Delete a job:
`kubectl delete job <job-name>`
*Deletes POD that houses the job as well


**Cron Jobs**

cron-definition.yaml
```yml
apiVersion: batch/v1beta1
kind: CronJob
metadata:
    name: <cronjob-name>
spec:
    schedule: "*/1 * * * *"
    jobTemplate:
        spec:
            [completions: 3] # run multiple jobs/pods one after the other
            [parallelism: 3] # run 3 jobs/pods in parallel
            template:
                spec:
                    containers:
                        - name: <name>
                          image: <image-name>
                    restartPolicy: Never         
```
Create cronjob:
`kubectl create -f cron-definition.yaml`
Display cronjobs:
`kubectl get cronjobs`
Check cronjob logs:
`kubectl logs <cronjob-name>`
Delete a cronjob:
`kubectl delete cronjob <cronjob-name>`
*Deletes POD that houses the job as well