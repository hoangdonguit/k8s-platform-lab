# Kyverno

This directory contains rendered Kyverno manifests generated from the upstream Helm chart plus a small policy baseline for the platform lab.

Reason for rendered-manifest mode:

- Keep Kyverno GitOps-managed and reproducible.
- Avoid relying on ArgoCD repo-server fetching external Helm repositories during reconciliation.
- Keep the deployment consistent with the rendered-manifest pattern used for ingress-nginx, cert-manager, MetalLB, kube-prometheus-stack, and Longhorn.

Chart:

- Repository: https://kyverno.github.io/kyverno/
- Chart: kyverno
- Version: 3.8.2
- App version: v1.18.2

Current lab configuration:

- Kyverno controllers run with 1 replica each.
- Policy reports are enabled.
- Initial policies use Audit mode.
- Baseline policies target the apps namespace only.
