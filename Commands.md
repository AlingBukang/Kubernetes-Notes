https://kubernetes.io/docs/reference/kubectl/conventions/

https://kubernetes.io/docs/reference/kubectl/cheatsheet


kubectl [command] [TYPE] [NAME] -o <output_format>

Here are some of the commonly used formats:

-o json Output a JSON formatted API object.
-o name Print only the resource name and nothing else.
-o wide Output in the plain-text format with any additional information.
-o yaml Output a YAML formatted API object.

Types:
pods, deployments, networkservices, services, ingress, logs

Display resources
`kubectl api-resources`

Check current user access
`kubectl auth can-i <command> <resource>`
`kubectl auth can-i create deployments`

Check access as another user
`kubectl auth can-i <command> <resource> --as <user>`

Count returned output
`kubectl get clusterroles --no-headers | wc -l`
`kubectl get clusterroles --no-headers -o json | jq '.items | length'`

Get
`kubectl get nodes`
`kubectl get All -A`
`k get all --all-namespaces`

Dry Run
`--dry-run=client -o yaml`

Run a command inside a POD
`kubctl exec -it <pod-name> [command]`

Get a shell
`kubctl exec -it <pod-name> -- sh`

Check network policy
`kubctl get netpol`

Label Nodes
`kubectl label nodes <node-name> <label-key>=<laber-value>`

POD
`kubectl run nginx --image=nginx --dry-run=client -o yaml`
Show labels
`kubectl get pod --show-labels`

Deployment
`kubectl create deployment --image=nginx nginx --dry-run -o yaml`

`kubectl create deployment nginx --image=nginx--dry-run=client -o yaml > nginx-deployment.yaml`

Service
Create a Service named redis-service of type ClusterIP to expose pod redis on port 6379

`kubectl expose pod redis --port=6379 --name redis-service --dry-run=client -o yaml`

Or

`kubectl create service clusterip redis --tcp=6379:6379 --dry-run=client -o yaml `(This will not use the pods labels as selectors, instead it will assume selectors as app=redis. You cannot pass in selectors as an option. So it does not work very well if your pod has a different label set. So generate the file and modify the selectors before creating the service)

Create a Service named nginx of type NodePort to expose pod nginx’s port 80 on port 30080 on the nodes:

`kubectl expose pod nginx --port=80 --name nginx-service --type=NodePort --dry-run=client -o yaml`

(This will automatically use the pod’s labels as selectors, but you cannot specify the node port. You have to generate a definition file and then add the node port in manually before creating the service with the pod.)

Or

`kubectl create service nodeport nginx --tcp=80:80 --node-port=30080 --dry-run=client -o yaml`

(This will not use the pods labels as selectors)

Both the above commands have their own challenges. While one of it cannot accept a selector the other cannot accept a node port. I would recommend going with the `kubectl expose` command. If you need to specify a node port, generate a definition file using the same command and manually input the nodeport before creating the service.
