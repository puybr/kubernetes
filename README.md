# Kubernetes Architecture
- Container orchestration managed by AWS
- Cluster → Nodes → Pods (wrappers around containers) and services
- Contains a master controller node and worker nodes
- Kubernetes (EKS) manages worker nodes only, the master controller node is managed by AWS
- The controller node is the hub that manages the cluster and contains the API server, etcd (datastore) and kube-scheduler used by kubernetes
- Persistent state of objects via the primary datastore etcd (distributed key-value store, stores and replicates all kubernettes cluster states)
- The controller manager is a daemon that embeds controllers inside the master node:
  - `replication`
  - `endpoints`
  - `namespace`
  - `serviceaccounts`
- The worker nodes talk to the master controller node through the API server → etcd → scheduler → kubelet → containerd (externally via kubectl to kubelet)
- kubelet runs containers on all nodes, defined by YAML called a pod manifest (read-only on port 10255), endpoints to curl: 
  - `/health`
  - `/pods`
  - `/spec`

# Logging
- Routes logs from worker nodes and sends logs

# Manifests
- Written in YAML

# Making pods
- YAML manifests or templates (using pods example)

# Services
- A service is like a load balancer
- Pods subscribe to a service and when we access pods we go through the service

# Namespaces
- Cluster namespaces (per environment):
  - `test`
  - `uat`
  - `prod`

# Kubectl 
- CLI tool for Kubernetes that connects to the controller node
- All communication is done through the Kubernetes API server
- Install the "kubectl" tool (needs AWS API keys configured to connect):
- `choco install kubernetes-cli`
- Connect to the EKS cluster:
- `aws eks --region eu-west-1 update-kubeconfig --name <cluster-name> --profile <profile-name>`
- Verify that you are connected to the correct cluster:
- `kubectl get pods -A`

- Get all deployments:
- `kubectl get deployment -A`

- Describe a deployment:
- `kubectl describe deployment <your-deployment> -n <namespace>`

- Scale the deployment down to 0:
- `kubectl scale --replicas=0 deployment/<your-deployment> -n <namespace>`

- Restart the deployment:
- `kubectl rollout restart deployment <your-deployment> -n <namespace>`

- Check the rollout status for the deployment:
- `kubectl rollout status deployment/<your-deployment> -n <namespace>`

- Check pod events:
- `kubectl get events -n <namespace> | grep -i <your-pod>`

- Check pod logs:
- `kubectl logs <your-pod> -n <namespace>`

- Delete pod:
- `kubectl delete pod <your-pod> -n <namespace>`

# Helm
- The Kubernetes package manager
- Install the "helm" package manager: 
  - `choco install kubernetes-helm`

- Clone the repo
- Verify that you are connected to the correct namespace:
  - `aws eks --region eu-west-1 update-kubeconfig --name <cluster-name> --profile <profile-name>`
- `cd helm/<chart>`
- `helm list -A`

- Show all information of the chart: 
- `helm show all ./`

- Upgrades a release to a new version of a chart:
- `helm upgrade <release-name> --install -f <env>-values.yaml --create-namespace --namespace=<namespace> ./<chart> --dry-run`

- Uninstalls the release:
- `helm uninstall <release-name> --dry-run`
