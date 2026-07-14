# Kyverno

This directory contains rendered Kyverno manifests generated from the upstream Helm chart plus a small policy baseline for the platform lab.

## Deployment model

The upstream Helm chart is rendered locally and committed into Git.

Render mode intentionally excludes Helm hooks and chart test resources.

Reason:

- Keep Kyverno GitOps-managed and reproducible.
- Avoid relying on ArgoCD repo-server fetching external Helm repositories during reconciliation.
- Avoid keeping one-shot Helm hook Jobs, such as migration Jobs, as long-lived GitOps resources.
- Keep the deployment consistent with the rendered-manifest pattern used for ingress-nginx, cert-manager, MetalLB, kube-prometheus-stack, and Longhorn.

## Chart

- Repository: https://kyverno.github.io/kyverno/
- Chart: kyverno
- Version: 3.8.2
- App version: v1.18.2

## Current lab configuration

- Kyverno controllers run with 1 replica each.
- Policy reports are enabled.
- Initial policies use Audit mode.
- Baseline policies target the apps namespace only.
- Baseline policies include Kyverno default fields to avoid unnecessary ArgoCD drift.

## Baseline policies

- p2-baseline-require-common-labels
- p2-baseline-disallow-privileged-apps
- p2-baseline-require-resource-requests-apps
