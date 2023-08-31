## Volumes

**Attach a volume to Pod**
```yml
apiVersion: v1
kind: Pod
metadata:
  name: webapp
spec:
  containers:
  - name: event-simulator
    image: kodekloud/event-simulator
    env:
    - name: LOG_HANDLERS
      value: file
    volumeMounts:
    - mountPath: /log
      name: log-volume

  volumes:
  - name: log-volume
    hostPath:
      # directory location on host
      path: /var/log/webapp
      # this field is optional
      type: Directory
```      


**Persistent Volume**

pv-definition.yml
```yml
apiVersion: v1
kind: PersistentVolume
metadata:
    name: pv-vol1
    labels:
      name: my-pv
spec:
    persistentVolumeReclaimPolicy: Retain   
    accessModes:
        - ReadWriteOnce
    capacity:
        storage: 1Gi
    [hostPath:              #for local storage
        path: /tmp/data]
    [awsElasticBlockStore:  #for AWS ebs
        volumeID: <volume-ID>
        fsType: ext4]    
```
`kubectl get persistentvolume`



**Persistent Volume Claims**

pvc-definition.yml
```yml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
    name: myclaim
spec:
    accessModes:
        - ReadWriteOnce
    resources:
        requests:
            storage: 500Mi 
    selector:
      matchLabels:
        name: my-pv
```
`kubectl get persistentvolumeclaim`

*Note:
PV and PVC should have the same access modes otherwise, they will not be bound but will not output any error.


**Using PVCs in PODs**

```yml
apiVersion: v1
kind: Pod
metadata:
  name: mypod
spec:
  containers:
    - name: myfrontend
      image: nginx
      imagePullPolicy: IfNotPresent
      volumeMounts:
      - mountPath: /var/www/html
        name: mypd
  volumes:
    - name: mypd
      persistentVolumeClaim:
        claimName: myclaim
```

## Storage

**Static Provisioning**
- provision a storage first before using

**Dynamic Provisioning**
- no need to initially provision a storage
- this can replcate provision volume

sc-definition.yml
```yml
apiVersion: storage.k8s.io/v1
kind: StorageClass
metadata:
  name: google-storage #using gcp
provisioner: kubernetes.io/gce-pd
volumeBindingMode: WaitForFirstConsumer 
parameters:
  type: [pd-standard | pd-ssd]
  replication-type: [none | regional-pd]
```

**Use storage class in PVC to replace PV**
- no need to provision PV

pvc-definition.yml
```yml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
    name: myclaim
spec:
    accessModes:
        - ReadWriteOnce
    storageClassName: google-storage    
    resources:
        requests:
            storage: 500Mi  
```
`kubectl get persistentvolumeclaim`




**Headless Service**
- like a normal service but has no IP of its own
- it doesn't do any load balancing
- it gives DNS to read each Pod using the Pod name and subdomain

headless-service.yml
```yml
apiVersion: v1
kind: Service
metadata:
  name: mysql-h
spec:
  ports:
    - port: 3306
  selector:
    app: mysql
  clusterIP: None      
```

**Use headless service in Pod**
*Use stateful sets instead

pod-definition.yml
```yml
apiVersion: v1
kind: Pod
metadata:
  name: myapp-pod
  labels:
    app: mysql
spec:
  containers:
  - name: mysql
    image: mysql
  subdomain: mysql-h #same name of headless service   
  hostname: mysql-pod #hostname of Pod   
```



## Stateful Sets

**Create set of pods with stateful sets**
statefulset-definition.yml
```yml
apiVersion: apps/v1
kind: StatefulSet
metadata:
  name: mysql
  labels:
    app: mysql

spec:
  replicas: 3
  selector:
    matchLabels:
      app: mysql
  template:
    metadata:
      labels:
        app: mysql
    spec:
      containers:
      - name: mysql
        image: mysql
        volumeMounts:
         - mountPath: /var/lib/mysql
           name: data-volume
  volumeClaimTemplates:
    metadata:
      name: myclaim
    spec:
      accessModes:
        - ReadWriteOnce
      storageClassName: google-storage    
      resources:
        requests:
            storage: 500Mi
  
  [servicename: mysql-h] #headless service        
  [podManagementPolicy: Parallel]         
```
**VolumeClaimTemplate**
- use for Stateful sets pvc provisioning so that each Pod will have a separate volume mount.

`kubectl create -f statefulset-definition.yml`
`kubectl scale statefulset mysql --replicas=5`



**Secret Volume**
secret-definition.yml
```yml
apiVersion: v1
kind: Pod
metadata:
  name: mypod
spec:
  containers:
  - name: mypod
    image: redis
    volumeMounts:
    - name: foo
      mountPath: "/etc/foo"
      readOnly: true
  volumes:
  - name: foo
    secret:
      secretName: mysecret
      optional: false # default setting; "mysecret" must exist
```