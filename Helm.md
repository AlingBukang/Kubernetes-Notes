
helm list
helm help
helm <repo>

Search specific charts:
`helm search hub <chart-name>`

Search repo:
`helm search repo <repo>`

Add a bitnami helm chart repository in the controlplane node:
`helm repo add bitnami https://charts.bitnami.com/bitnami`

Download package:
`helm pull --untar <bitnami/apache>`

Install downloaded package:
`helm install mywebapp ./apache`