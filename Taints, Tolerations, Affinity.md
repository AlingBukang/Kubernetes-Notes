
A taint is not schedule on a master automatically but can be modified although not a best practice.

Taints are set on Nodes
Tolerants are set on Pods


**Taints**
`kubectl taint nodes <node-name> key=value:taint-effect`

**Taint Effects**
* what happens to the PODs that do not tolerate taint effect set
- NoSchedule - will not schedule PODs to Node at all
- PreferNoSchedule - will try not to schedule PODs on Node but no guarantee
- NoExecute - will evict PODs that are already schedule in the Node


**Tolerations**
pod-definition.yml
```yml
apiVersion:
kind:
metadata:
    name: <name>
spec:
    containers:
    - name: <container-name>
      image: <image-name>
    tolerations:
    - key: <"key-name">
      operator: "Equal"
      value: <"value"> 
      effect: ["NoSchedule"|"PreferNoSchedule"|"NoExecute"]
```

**Node Affinity**
pod-definition.yml
```yml
apiVersion:
kind:
metadata:
    name: <name>
spec:
    containers:
    - name: <container-name>
      image: <image-name>
    affinity:
      nodeAffinity:
        requiredDuringSchedulingIgnoredDuringExecution:
          nodeSelectorTerms:
          - matchExpressions:
            - key: <size>
              operator: [In | NotIn | Exists]
              [values:
              - <Large>]
```

**Node Affinity Types**
Available:
1. requiredDuringSchedulingIgnoredDuringExecution
2. preferedDuringSchedulingIgnoredDuringExecution

Planned:
3. requiredDuringSchedulingRequiredDuringExecution

----------------------------------------------
       | DuringScheduling   | DuringExecution
----------------------------------------------
Type 1 | Required           | Ignored
Type 2 | Preferred          | Ignored
Type 3 | Required           | Required

`Type 1` - A pod will not be scheduled if no match selector(label) but will continue to run if already scheduled in a Node
`Type 2` - A pod will not be scheduled if no match selector(label) unless there are no other Nodes that to schedule that pod into then it will be scheduled on a Node on best effort basis. It will continue to run if already scheduled in a Node.
`Type 3` - A pod will not be scheduled if no match selector(label) and it will be evicted from the Node if it already exists there.