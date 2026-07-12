# Longhorn

This directory contains rendered Longhorn manifests generated from the upstream Helm chart.

Reason for rendered-manifest mode:

- Keep Longhorn GitOps-managed and reproducible.
- Avoid relying on ArgoCD repo-server fetching external Helm repositories during reconciliation.
- Keep the deployment consistent with the rendered-manifest pattern used for ingress-nginx, cert-manager, MetalLB, and kube-prometheus-stack.

Chart:

- Repository: https://charts.longhorn.io
- Chart: longhorn
- Version: 1.12.0
- App version: v1.12.0

Current lab configuration:

- Longhorn UI is ClusterIP only.
- Longhorn is not configured as the default StorageClass.
- PVC tests should explicitly use storageClassName: longhorn.
- Replica count: 3.
- Data path: /var/lib/longhorn.
