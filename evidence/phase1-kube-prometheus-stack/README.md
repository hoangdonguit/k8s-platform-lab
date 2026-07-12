# Phase 1 - kube-prometheus-stack Evidence

## Goal

Deploy a GitOps-managed monitoring stack for the K3s platform lab.

## Result

PASS.

The kube-prometheus-stack application was deployed through ArgoCD and reached `Synced` / `Healthy`.

## Components

- Prometheus Operator
- Prometheus
- Alertmanager
- Grafana
- kube-state-metrics
- node-exporter
- ServiceMonitor CRDs
- PrometheusRule CRDs

## Deployment model

The upstream Helm chart was rendered locally and committed into Git.

Reason:

- Keep the monitoring stack reproducible.
- Avoid depending on ArgoCD repo-server fetching external Helm repositories during reconciliation.
- Keep the deployment consistent with the rendered-manifest pattern used for ingress-nginx, cert-manager, and MetalLB.

## Chart

- Repository: `https://prometheus-community.github.io/helm-charts`
- Chart: `kube-prometheus-stack`
- Chart version: `87.15.1`
- App version: `v0.92.1`

## K3s-specific choices

Some control-plane monitors were disabled for the MVP baseline:

- kube-controller-manager
- kube-scheduler
- kube-proxy
- etcd

Reason:

K3s exposes or packages these components differently from a kubeadm-style cluster. Disabling them avoids noisy false-down targets in the first monitoring baseline.

## Validation

The following checks passed:

- ArgoCD app `p2-kube-prometheus-stack`: `Synced` / `Healthy`
- Prometheus pod: `2/2 Running`
- Alertmanager pod: `2/2 Running`
- Grafana pod: `3/3 Running`
- Prometheus Operator deployment: `1/1 Available`
- kube-state-metrics deployment: `1/1 Available`
- node-exporter DaemonSet: `3/3 Ready`
- Monitoring CRDs installed
- No non-running pods in the cluster
- Internal service checks passed for:
  - Prometheus readiness endpoint
  - Alertmanager readiness endpoint
  - Grafana health endpoint
  - Prometheus `up` query
- Grafana dashboard access was verified through local port-forward.

## Evidence files

- `raw/argocd-applications.txt`
- `raw/monitoring-resources.txt`
- `raw/monitoring-crds.txt`
- `raw/servicemonitors.txt`
- `raw/prometheusrules.txt`
- `raw/prometheus-alertmanager-custom-resources.yaml`
- `raw/monitoring-core-services.yaml`
- `raw/functional-checks.txt`
- `raw/non-running-pods.txt`
