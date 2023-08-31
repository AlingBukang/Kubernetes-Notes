
k create ns dev-ns
k get namespaces
k get pods --namespace=<namespace>
kubectl get pods --all-namespaces
k run redis --image=redis --namespace=finance