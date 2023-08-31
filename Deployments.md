`deployment-definition.yml`

```yml
apiVersion: apps/v1
kind: Deployment
metadata:
    name: myapp-deployment
    labels:
        app: myapp
        type: front-end
spec:
  template:
      metadata:
        name: my-app-pod
        labels:
          app: myapp
          type: front-end
      spec:
        containers:
        - name: [String]
          image: [image-name]
  replicas: [# of replicas to create]
  selector:
    matchLabels:
      [label-name]: [label-value]
  strategy:
    rollingUpdate:
      maxSurge: 25%
      maxUnavailablie: 25%
    type: RollingUpdate             
```

**To create**
`kubectl create -f deployment-definition.yml [--record]`
**To view controllers**
`kubectl get deployments`
`kubectl describe deployments`

The deployment, when run, will automatically create a replicaset.

`kubectl get replicaset`
`kubectl get pods`
or 
`kubectl get all`

**Deployment Rollouts**
`kubectl rollout status deployment/myapp-deployment`
`kubectl rollout history deployment/myapp-deployment`

**To Rollback**
`kubectl rollout undo deployment/myapp-deployment`

**Deployment Strategies**
- Recreate (all at once)
- Rolling update
  
Default deployment stategy: rolling update

**2 Ways to Update Deployment**
Update definition file then run:
`kubectl apply -f deployment-definition.yml`
OR
`kubectl set image deploy [deployment-name] [container-name]=[image]`
OR
`kubectl edit deployment [deployment-name]
