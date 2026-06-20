# argo-git-ops

This repository is the GitOps source of truth for ArgoCD.

Jenkins renders the `hello-newapp` Helm chart and updates
`dev/devops-template.yaml`, `qa/devops-template.yaml`, or
`prod/devops-template.yaml`, depending on the selected pipeline environment.

The ArgoCD ApplicationSet in `applicationset.yaml` creates one Application per
environment and deploys the rendered Kubernetes manifests from those folders.

Environment folders map to Kubernetes namespaces as follows:

- `dev` -> `hello-newapp-dev`
- `qa` -> `hello-newapp-qa`
- `prod` -> `hello-newapp-prod`

## Monitoring

The `monitoring/` folder contains ArgoCD Applications for Prometheus and
Grafana. They install the monitoring stack into the `monitoring` namespace.

Prometheus scrapes the `hello-newapp` Service through its `/metrics` annotations.
Grafana is configured with Prometheus as a data source and includes the
`hello-newapp-monitoring` dashboard.

### Grafana Dashboard

The Grafana dashboard was created in the Grafana UI, exported, and added to
`monitoring/grafana.yaml`.

This means Grafana starts with the dashboard already configured, instead of
recreating it manually every time.

Apply the monitoring Applications once:

```bash
kubectl apply -f monitoring/prometheus.yaml
kubectl apply -f monitoring/grafana.yaml
```
