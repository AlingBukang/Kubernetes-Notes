`pod-definition.yml`

```yml
apiVersion: [String]
kind: [Pod|Service|ReplicaSet|Deployment]
metadata:
    name: [String]
    labels:
        app: [String]
        type: [String]
spec:
    containers:
    - name: [String]
      image: [image-name]
```

**To create**
`kubectl create -f pod-definition.yml`

**To run:**
`kubectl apply -f pod-definition.yml`
`kubectl get pods`
`kubectl describe pod myapp-pod`

**To edit POD properties**
`kubectl edit pod <pod-name>`

`kubectl edit pod <pod-name> -o yaml > new-pod.yaml`

`kubectl edit pod <pod-name> -n <namespace>`

`kubectl replace --force -f /tmp/updated-config.yaml`



Sample POD definition:

```yml
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:
    run: my-busybox
  name: my-busybox
  namespace: dev2406
spec:
  volumes:
  - name: secret-volume
    secret:
      secretName: dotfile-secret
  nodeSelector:
    kubernetes.io/hostname: controlplane
  tolerations:
  - key: "node-role.kubernetes.io/control-plane"
    operator: "Exists"
    effect: "NoSchedule"
  containers:
  - command:
    - sleep
    args:
    - "3600"
    image: busybox
    name: secret
    volumeMounts:
    - name: secret-volume
      readOnly: true
      mountPath: "/etc/secret-volume"
```