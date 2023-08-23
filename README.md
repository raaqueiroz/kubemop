# Kubemop
A Helm chart for Kubernetes to provision a CronJob and RBAC to clean PODs with specific Statuses

## Referências
- https://github.com/raaqueiroz/kubemop

## Instalação

```sh
helm repo add kubemop https://raaqueiroz.github.io/kubemop/
helm install kubemop kubemop/kubemop -f values.yaml -n kube-system
```

## Customizações do values.yaml

| Parâmetro  | Valor default | Valor custom | Motivo |
|------------|---------------|--------------|--------|
| `cronJob.cronExpression` | `"3 */3 * * *"` | `"5 */3 * * *"` | Alterado o minuto para executar a limpeza após a execução de outros PODs e jobs do cluster |
| `cronJob.excludedNamespaces` | `[]` | `["kube-system", "istio-system"]` | Adicionada namespaces para bypass e não efetuar nenhuma ação de exclusão |
| `jobTemplate.env` | `[]` | `["DD_AGENT_HOST", "DD_ENV", "DD_SERVICE", "DD_VERSION"]` | Variáveis padrão para uso do Datadog |
| `jobTemplate.nodeSelector` | `{}` | `true` | Habilitar a coleta de logs dos containers |
| `jobTemplate.tolerations` | `[]` | `[ { "key": "CriticalAddonsOnly", "operator": "Exists" } ]` | Toleration para permitir que o POD seja executado no node com taint de addons |
| `jobTemplate.podLabels` | `{}` | `{ "tags.datadoghq.com/env": "qa", "tags.datadoghq.com/service": "kubemop", "tags.datadoghq.com/version": 0.3.0 }` | Annotations padrão para uso do Datadog |
