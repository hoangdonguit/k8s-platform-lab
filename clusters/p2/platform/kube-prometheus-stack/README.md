# kube-prometheus-stack

This directory contains rendered kube-prometheus-stack manifests generated from the upstream Helm chart.

Reason for rendered-manifest mode:

- Keep the monitoring stack GitOps-managed and reproducible.
- Avoid relying on ArgoCD repo-server fetching external Helm repositories at reconciliation time.

Chart:

- Repository: https://prometheus-community.github.io/helm-charts
- Chart: kube-prometheus-stack
- Version: 87.15.1
- App version: v0.92.1

Current lab configuration:

- Prometheus Operator enabled.
- Prometheus enabled with 1 replica.
- Alertmanager enabled with 1 replica.
- Grafana enabled as ClusterIP.
- kube-state-metrics enabled.
- node-exporter enabled.
- kubelet and CoreDNS monitoring enabled.
- kube-controller-manager, kube-scheduler, kube-proxy, and etcd monitors disabled for this K3s lab baseline.
