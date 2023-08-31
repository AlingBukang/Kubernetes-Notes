## Ingress Controller

Sample ingress controller: Nginx
Required:
- deployment
- service
- configMap
- auth

ingress-controller-definition.yaml
```yml
apiVersion: apps/v1
kind: Deployment
metadata:
    name: nginx-ingress-controller
spec:
    replicas: 1
    selector:
        matchLabels:
            name: nginx-ingress
    template:
        metadata:
            labels:
                name: nginx-ingress
        spec:
            containers:
                - name: nginx-ingress-controller
                  image: <nginx-controller-image>            
                  args:
                    - /nginx-ingress-controller
                    - --configmap=$(POD_NAMESPACE)/nginx-configuration
                  env:
                    - name: POD_NAME
                      valueFrom:
                        fieldRef:
                          fieldPath: metadata.name
                    - name: POD_NAMESPACE
                      valueFrom:
                        fieldRef:
                          fieldPath: metadata.namespace 
                  ports:
                    - name: http
                      containerPort: 80
                    - name: https
                      containerPort: 443                         
```

nginx-config-map.yaml
```yml
apiVersion: v1
kind: ConfigMap
metadata:
    name: nginx-configuration
```

nginx-service-configuration.yaml
```yml
apiVersion: v1
kind: Service
metadata:
    name: nginx-ingress
spec:
  type: NodePort
  ports:
  - port: 80
    targetPort: 80
    protocol: TCP
    nodePort: <30000>
    name: http
  - port: 443
    targetPort: 443
    protocol: TCP
    name: https
  selector:
    name: nginx-ingress  
```
OR create service via cli
`kubectl expose deploy <deploy-name> -n <namespace>  --name <name> --port=<port> --target-port=<target-port>`


nginx-service-account.yaml
```yml
apiVersion: v1
kind: ServiceAccount
metadata:
    name: nginx-ingress-serviceaccount
```



## Ingress Resources
- set of rules and configurations applied to ingress controller, ex. routing

ingress-webapp-config.yaml
```yml
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
    name: ingress-webapp
    namespace: <namespace>
    annotations:
      nginx.ingress.kubernetes.io/rewrite-target: /
spec:
    backend:
        serviceName: webapp-service
        servicePort: 80
```

`kubectl create -f ingress-webapp-config.yaml`
`kubectl get ingress`

**Create rule definition by host:**

ingress-webapp-rules.yaml
```yml
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
    name: ingress-webapp-route
    namespace: <namespace>
    annotations:
      nginx.ingress.kubernetes.io/rewrite-target: /
spec:
  rules:
  - host: route1.webapp.com
    http:
      paths:
      - backend:
          serviceName: webapp-service
          servicePort: 80
  - host: route2.webap.com
    http:
      paths:
      - backend:
          serviceName: route2-service
          servicePort: 80   
```

**Create rule definition by path:**
*New K8s version format

ingress-webapp-rules.yaml
```yml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
    name: ingress-webapp-route
    namespace: <namespace>
    annotations:
      nginx.ingress.kubernetes.io/rewrite-target: /
spec:
  rules:
  - http:
      paths:
      - path: /route1
        pathType: Prefix
        backend:
          service:
            name: route1-service
            port:
              number: 80
      - path: /route2
        pathType: Prefix
        backend:
          service:
            name: route2-service
            port:
              number: 80        