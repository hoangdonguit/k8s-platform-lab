# cert-manager

This directory contains rendered cert-manager manifests generated from the upstream Helm chart.

Reason for rendered-manifest mode:

- ArgoCD repo-server previously failed to fetch external Helm repositories reliably.
- To keep deployment GitOps-managed and reproducible, the chart is rendered locally and committed into this repository.
- ArgoCD applies manifests from GitHub instead of pulling the Helm chart at reconciliation time.

Chart:

- Repository: https://charts.jetstack.io
- Chart: cert-manager
- Version: v1.20.3

Initial issuer:

- selfsigned-cluster-issuer

Notes:

- This is a lab bootstrap issuer, not a production public CA issuer.
- Let's Encrypt/ACME can be added later after domain, DNS and ingress exposure strategy are decided.
