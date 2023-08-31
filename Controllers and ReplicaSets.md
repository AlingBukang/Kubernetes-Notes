Replication Controller and ReplicaSet
- handles HA
- load balancing
- scaling

ReplicationController - old version of ReplicaSet
Main difference:
ReplicaSet can manage pods that were not created by the replicaset via the selector(specifies what pod falls under the replicaset)

Sample `rc-definition.yml`:

```yml
apiVersion: v1
kind: ReplicationController
metadata:
    name: myapp-rc
    labels:
        app: myapp
        type: front-end
spec:
  template:
      metadata:
        name: [String]
        labels:
          app: [String]
          type: [String]
      spec:
        containers:
        - name: [String]
          image: [image-name]
  replicas: [# of replicas to create]         
```

**To create**
`kubectl create -f rc-definition.yml`
**To view controllers**
`kubectl get replicationcontroller`


Sample `replicaset-definition.yml`:

```yml
apiVersion: apps/v1
kind: ReplicaSet
metadata:
    name: myapp-replicaset
    labels:
        app: myapp
        type: front-end
spec:
  template:
      metadata:
        name: [String]
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
```

**To create**
`kubectl create -f replicaset-definition.yml`
**To view controllers**
`kubectl get replicaset`

**To scale replicas**
Change the `replicas` value to desired scale in the `yml` file. Then, run:
`kubectl replace -f replicaset-definition.yml`
_This will update the # of replicas to the desired #

**Ways to scale using cli**
`kubectl scale --replicas=6 -f replicaset-definition.yml`
OR specify the replicaset name:
`kubectl scale --replicas=6 replicaset myapp-replicaset`
OR edit the yml file via cli(vim):
`kubectl edit replicaset myapp-replicaset`
*this auto executes upon save

**View replicaset**
`kubectl describe replicaset myapp-replicaset`