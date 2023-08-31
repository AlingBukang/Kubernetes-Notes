#vimtutor

#export do="--dry-run=client -o yaml" so you can use "k run nginx $do > nginx.yaml"

pod - contains 1 container or more of different kinds
Nodes - aka minions, workers
Cluster - compose of nodes
kubectl - command line tool

Kube Components:
- API server(kube-apiserver)
- etcd
- kubelet
- container runtime
- controller
- scheduler

2 Types of servers:
- master has kube-apiserver, etcd, controller, scheduler
- worker nodes has containers, kubelet

Kubectl commands:
kubectl run [image-name] --image image-name
kubectl cluster-info
kubectl get nodes -o wide
kubectl get pods -o wide

Delete a pod:
kubectl delete pod [pod-name]


Setup:
For local devs:
- minikube
- microk8s
- kubeadm
