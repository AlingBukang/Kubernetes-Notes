
**Patterns**
- Ambassador
- Adapter
- Sidecar

Examples:
- Sidecar - web server + log agent
- Ambassador - 


`initContainer`
- a temporary task that will be run only one time when the pod is first created or a process that waits for an external service or database to be up before the actual application starts.

- each init container is run one at a time in sequential order.

- initContainer is configured in a pod like all other containers, except that it is specified inside a initContainers section, like this:

pod-definition.yaml
```yml
apiVersion: v1
kind: Pod
metadata:
  name: myapp-pod
  labels:
    app: myapp
spec:
  containers:
  - name: myapp-container
    image: busybox:1.28
    command: ['sh', '-c', 'echo The app is running! && sleep 3600']
  - name: log-container
    image: log-image
  initContainers:
  - name: init-myservice
    image: busybox
    command: ['sh', '-c', 'git clone <some-repository-that-will-be-used-by-application> ;']
```