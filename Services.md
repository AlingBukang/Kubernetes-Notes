### NodePort 
- valid ports 30000-32767

Node -> has:
- POD -> has:
  - IP
  - TargetPort
  - -> is:
    - connected to Service via IP and port
- Service -> has:
  - IP
  - Port
  - -> is:
    - connected to one or more POD via IP and port
    - to distinguish POD connections, tagging/labeling(`selector`) is used
    - connected to NodePort via port
    - port is an array
- NodePort
  - has port ranging from 30000-32767
  - is used to connect outside the Node
  



service-definition.yml
```yml
apiVersion: v1
kind: Service
metadata:
    name: myapp-service
spec:    
    type: NodePort
    ports:
    - targetPort: 80
      port: 80
      nodePort: [30000-32767]
    selector:
      app: myapp
      type: front-end
```

**Create service**
`kubectl create -f service-definition.yml`
`kubectl get services`
`kubectl describe service`

**To run in minikube(local)**
`minikube service <service-name> --url http://Node-IP:Node-Port`

To access the webserver:
`curl http://Node-IP:Node-Port`

**Auto-scaling: multiple PODs in a Node**
- the Service will look for all target PODs via selector(labels) based on the request from the client
- load balancing across the target PODs will be done by the Service automatically using the labels
- algorithm used:
  - Algorithm: Random
  - SessionAffinity: Yes

**Auto-scaling: multiple Nodes in a cluster**
- Service handles load balancing behind the scene; there's is no need for additional configuration
- any Node IP can be used to access all target Nodes in the cluster

### ClusterIP
Ex. connect services together, e.g. front-end cluster, back-end cluster, redis, etc.

service-definition.yml
```yml
apiVersion: v1
kind: Service
metadata:
    name: back-end
spec:    
    type: ClusterIP #default type
    ports:
    - targetPort: 80
      port: 80
    selector:
      app: myapp
      type: back-end
```

**Create service**
`kubectl create -f service-definition.yml`
`kubectl get services`
`kubectl describe service`

The cluster can be accessed by other PODs using the clusterIP or app name.


### Load Balancer
Utilise cloud native LB from AWS, GCP, Azure, etc. to provide elastic IP, multicast, dns

service-definition.yml
```yml
apiVersion: v1
kind: Service
metadata:
    name: myapp-service
spec:    
    type: LoadBalancer
    ports:
    - targetPort: 80
      port: 80
      nodePort: 30008
    selector:
      app: myapp
      type: back-end
```

**Expose a deployment**
`kubectl expose deployment redis --port=6379 --name messaging-service --namespace marketing`