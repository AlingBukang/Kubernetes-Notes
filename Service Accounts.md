

**Create service account**
`kubectl create serviceaccount <service-account-name>`

**Create a token for the service account**
`kubectl create token <service-account-name>`

**Add service account in pod definition file**
```yml
spec:
    serviceAccountName: dashboard-sa
```


**To create a non-expiring token**
secret-definition.yml
```yml
apiVersion: v1
kind: Secret
type: kubernetes.io/service-account-token
metadata:
    name: <secret-name>
    annotations:
        kubernetes.io/service-account.name: <service-account-name>