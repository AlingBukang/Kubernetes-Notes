`kubectl -n <namespace> logs kibana`
`kubectl logs -f <pod-name> [container-name]`

**Readiness Probes**

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
  #http test  
  readinessProbe:
    httpGet:
        path: /api/ready
        port: 8080
    initialDelaySeconds: 10 # set delay
    periodSeconds: 5    # how often
    failureThreshold: 5 # times to run
  #socket test
  readinessProbe:
    httpGet:
        path: /api/ready
        port: 8080
  #exec command
  readinessProbe:
    exec:
        command: 
            - cat
            - /app/is_ready        
```


**Liveness Probes**
- solves issue: container is running but has error
- periodically checks if a container is healthy

```yml
<SNIP>
spec:
  containers:
  - name: myapp-container
    image: busybox:1.28
    command: ['sh', '-c', 'echo The app is running! && sleep 3600']
  #http test  
  livenessProbe:
    httpGet:
        path: /api/healthy
        port: 8080
    initialDelaySeconds: 10 # set delay
    periodSeconds: 5    # how often
    failureThreshold: 5 # times to run
  #socket test
  livenessProbe:
    httpGet:
        path: /api/healthy
        port: 8080
  #exec command
  livenessProbe:
    exec:
        command: 
            - cat
            - /app/is_healthy 
```

**Monitoring**
`kubectl top node`  
`kubectl top pod`  

Build sample metrics:
```sh
git clone https://github.com/kodekloudhub/kubernetes-metrics-server.git

cd kubernetes-metrics-server/
k create -f .
```