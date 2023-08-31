Check network policy
`kubectl get netpol`

Edit netpol
`kubectl get netpol <netpol-name> -oyaml > new-netpol.yaml`

Get a shell
`kubctl exec -it <pod-name> -- sh`
Check service
`nc -vzw 2 <service-name> <port>`



Create DB policy that only accepts traffic from api-pod and serve output to an external backup server:
`network-policy-definition.yml`

```yml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
    name: db-policy
    namespace: <ns-name>
spec:
    podSelector:
        matchLabels:
            <role: db>
    policyTypes:
    - Ingress
    - Egress
    ingress:
    - from:
      - podSelector:
          matchLabels:
            name: api-pod
        namespaceSelector:
          matchLabels:  
            name: <target-namespace>
      - ipBlock:  # specify IP range instead of or and podSelector
          cidr: <IP/32>        
      ports:
      - protocol: TCP
        port: 3306
    egress:
    - to:
      - ipBlock:
            cidr: [cidr-range]
      ports:
      - protocol: TCP
        port: 80                  
```

`kubectl create -f network-policy-definition.yml`