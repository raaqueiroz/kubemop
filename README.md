# Kubemop
A Helm chart for Kubernetes to provision a CronJob and RBAC to clean PODs with specific Statuses

## Installation

```sh
helm repo add kubemop https://raaqueiroz.github.io/kubemop/
helm install kubemop kubemop/kubemop -n kube-system
```

## Customizations

| Parameter  | Default value | Sample custom | Description |
|------------|---------------|--------------|--------|
| `cronJob.cronExpression` | `"3 */3 * * *"` | `"5 4 */2 * *"` | Define recurrency of kubemop runs |
| `cronJob.excludedNamespaces` | `[]` | `["kube-system", "istio-system"]` | List of namespaces you want to exclude from cleanup |
| `cronJob.additionalPodStatuses` | `["Error", "Evicted", "ContainerStatusUnknown", "NotReady", "OOMKilled", "Completed", "Terminating", "ImagePullBackOff", "ErrImagePull"]` | `["CrashLoopBackOff"]` | Additional statuses you want to append to default list |
| `jobTemplate.nodeSelector` | `{}` | `{ "CriticalAddonsOnly": "true" }` | Node selector to look up for specific node |
| `jobTemplate.tolerations` | `[]` | `[ { "key": "CriticalAddonsOnly", "operator": "Exists" } ]` | Toleration to allow scale POD in specific work node |
