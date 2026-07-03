# ingress-nginx

This directory contains rendered ingress-nginx manifests generated from the upstream Helm chart.

Reason for rendered-manifest mode:

- The ArgoCD repo-server in the cluster failed to fetch the external Helm repo directly.
- The observed error was: `tls: first record does not look like a TLS handshake`.
- To keep the deployment GitOps-managed and reproducible, the chart is rendered locally and committed into this repository.
- ArgoCD applies the manifests from GitHub instead of pulling the Helm chart from the external repo at reconciliation time.

Chart:

- Repository: https://kubernetes.github.io/ingress-nginx
- Chart: ingress-nginx
- Version: 4.15.1

Access mode:

- Controller kind: DaemonSet
- Service type: NodePort
- HTTP NodePort: 30080
- HTTPS NodePort: 30443
