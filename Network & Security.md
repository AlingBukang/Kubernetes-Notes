Note:
Capabilities are only supported at the container level and not at he POD level

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
      command: ["sleep", "1000"]
      securityContext:
        runAsUser: 1000
        capabilities:
            add: ["MAC_ADMIN"]

spec:
  securityContext:
    runAsUser: 1001
  containers:
  -  image: ubuntu
     name: web
     command: ["sleep", "5000"]
     securityContext:
      runAsUser: 1002

  -  image: ubuntu
     name: sidecar
     command: ["sleep", "5000"]
```