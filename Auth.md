Check auth modes on the cluster:`kubectl describe pod kube-apiserver-controlplane -n kube-system`




**Authorization Mechanisms**
- Node - handled by node authoriser; access within a cluster
- ABAC - attribute-based access control
- RBAC - role-based access control
- Webhook



**Role Based Access Controls**
1. Create a role
developer-role.yaml
```yml
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
    name: developer
rules:
- apiGroups: [""]
  resources: ["pods"]
  [resourceName: ["resource-name1"]] # give granular resource ex. specific pod name
  verbs: ["list", "get", "create", "update", "delete"]
```
`kubectl create -f developer-role.yaml`

2. Bind a user to the role
devuser-developer-binding.yaml
```yml
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
    name: devuser-developer-binding
subjects:
- kind: User
  name: dev-user
  apiGroup: rbac.authorization.k8s.io
roleRef:
  kind: Role
  name: developer
  apiGroup: rbac.authorization.k8s.io
  ```
  `kubectl create -f devuser-developer-binding.yaml`


## Cluster Roles

cluster-admin-role.yaml
```yml
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRole
metadata:
    name: cluster-administrator
rules:
- apiGroups: [""]
  resources: ["nodes"]
  verbs: ["list", "get", "create", "update", "delete"]
```

cluster-admin-role-binding.yaml
```yml
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
    name: cluster-admin-role-binding
subjects:
- kind: User
  name: cluster-admin
  apiGroup: rbac.authorization.k8s.io
roleRef:
  kind: ClusterRole
  name: cluster-administrator
  apiGroup: rbac.authorization.k8s.io
  ```