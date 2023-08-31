**KubeConfig**

KubeConfig file:
# $HOME/.kube/config

```yml
apiVersion: v1
kind: Config

current-context: test-admin@my-kube-test #default user/context

clusters: #environments
- name: my-kube-sandbox
  cluster:
    certificate-authority: ca.crt
    server: https://my-kube-sandbox:6443
- name: my-kube-test
  cluster:
    certificate-authority: ca.crt
    server: https://my-kube-test:6443

contexts:
- name: my-kube-admin@my-kube-sandbox
  context:
    cluster: my-kube-sandbox
    user: my-kube-admin
    [namespace: <name>]
- name: test-admin@my-kube-test
  context:
    cluster: my-kube-test
    user: test-admin    

users:
- name: my-kube-admin
  user:
    client-certificate: admin.crt
    client-key: admin.key
- name: test-admin
  user:
    client-certificate: admin.crt
    client-key: admin.key    
```

Display config on current user's home dir:
`kubectl config view` 

To specify a config:
`kubectl config view --kubeconfig=<config-name>`

To change context:
`kubectl config [--kubeconfig=<config-name>] use-context <context-name>`

To know the current context: 
`kubectl config [--kubeconfig=<config-name>] current-context`