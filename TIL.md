Docker & Kubernetes commands:

- command in K8s overrides ENTRYPOINT in Docker
- args in Ks=8s overrides CMD in Docker

**Note on editing PODs and Deployments**

**Edit Deployments**

Remember, you CANNOT edit specifications of an existing POD other than the below.

    spec.containers[*].image
    spec.initContainers[*].image
    spec.activeDeadlineSeconds
    spec.tolerations

**Workaround 1:**
Run `kubectl edit pod <pod name>`. This will open the pod specification in an editor (vi editor). Then edit the required properties. When you try to save it, you will be denied. This is because you are attempting to edit a field on the pod that is not editable.

A copy of the file with your changes is saved in a temporary location.
You can then delete the existing pod by running the command:

`kubectl delete pod webapp`

Then create a new pod with your changes using the temporary file:

`kubectl create -f /tmp/kubectl-edit-ccvrq.yaml`

**Workaround 2:**
The second option is to extract the pod definition in YAML format to a file using the command:
`kubectl get pod webapp -o yaml > my-new-pod.yaml`

Then make the changes to the exported file using an editor (vi editor). Save the changes:
`vi my-new-pod.yaml`

Then delete the existing pod:
`kubectl delete pod webapp`

Then create a new pod with the edited file
`kubectl create -f my-new-pod.yaml`

**Edit Deployments**

With Deployments, any field/property of the POD template can be easily edited since the pod template is a child of the deployment specification; every change on the deployment will automatically delete and create a new pod with the new changes.
`kubectl edit deployment my-deployment`