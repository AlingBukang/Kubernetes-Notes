

Admission Controllers:
- AlwaysPullImages
- NodeRestriction
- NamespaceAutoProvision
- DefaultStorageClass
- EventRateLimit
- NamespaceLifecycle
- many more....


View enabled Admission Controllers
`kubectl exec kube-apiserver-controlplane -n kube-system -- kube-apiserver -h | grep enable-admission-plugins`

`ps -ef | grep kube-apiserver | grep admission-plugins`

Enable Admission controllers
`cat /etc/kubernetes/manifests/kube-apiserver.yaml`

`/etc/kubernetes/manifests/kube-apiserver.yaml`
Add on spec.containers.command:
- --enable-admission-plugins=NodeRestriction,NamespaceAutoProvision