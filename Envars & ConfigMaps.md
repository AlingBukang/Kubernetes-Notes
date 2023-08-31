**Env Variables & Config Maps**
- `env` are array of key-pair values

```yml
env:
   - name: name
     value: value

env:
   - name: name
     valueFrom: 
        configMapRef:

   - name: name
     valueFrom: 
        configMapKeyRef:        

env:
   - name: name
     valueFrom:
        secretKeyRef:  
        
volumes:
- name: <volume-name>
  configMap:
    name: <config-name>        
```     

**Create ConfigMaps**
Imperative way:
`kubectl create cm <config-name> --from-literal=<key>=<value>`

`kubectl create cm <config-name> --from-file=<path-to-file>`

Declarative way:
config-map.yaml
```yml
apiVersion: v1
kind: ConfigMap
metadata:
    name: app-config
data:
    <key>: <value>    
```

`kubectl create -f config-map.yaml`


**ConfigMap in PODs**
`config-map.yaml`
```yml
apiVersion: v1
kind: ConfigMap
metadata:
    name: app-config
data:
    <key1>: <value1>    
    <key2>: <value2>  
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
        - configMapRef:
            name:  app-config
```
