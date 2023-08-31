```yml
envFrom:
   - secretRef:
        name: <name> 

env:
   - name: name
     valueFrom: 
        secretKeyRef:
            name: <name>
            key: <value>
        
volumes:
- name: <volume-name>
  secret:
    secretName: <secret-name>        
```     


**Create Secrets**   
Imperative way:
`kubectl create secret generic <secret-name> --from-literal=<key>=<value>`

`kubectl create secret generic <secret-name> --from-file=<path-to-file>`

Declarative way:
secret-data.yaml
```yml
apiVersion: v1
kind: Secret
metadata:
    name: <secret-name>
data:
    <key>: <base64-encoded-value>    
```

`kubectl create -f secret-data.yaml`


**Secrets in PODs**
`secret-data.yaml`
```yml
apiVersion: v1
kind: Secret
metadata:
    name: app-secret-config
data:
    DB_HOST: <base64-encoded-value>     
    DB_Password: <base64-encoded-value>  
```


`pod-definition.yml`
```yml
apiVersion: v1
kind: Pod
metadata:
    name: <String>
    labels:
        name: <String>
spec:
    containers:
    - name: <String>
      image: <image-name>
      ports:
        - containerPort: <Port>
      envFrom:
        - secretRef:
            name:  app-secret-config
```

`kubectl create -f pod-definition.yml`


**Encrypt data at rest**
https://kubernetes.io/docs/tasks/administer-cluster/encrypt-data/

