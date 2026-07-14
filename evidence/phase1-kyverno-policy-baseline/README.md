# Phase 1 - Kyverno Policy Baseline Evidence

## Goal

Deploy Kyverno as the policy-as-code and admission control layer for the K3s platform lab.

## Result

PASS.

Kyverno was deployed through ArgoCD and reached `Synced` / `Healthy`.

## Components validated

- Kyverno admission controller
- Kyverno background controller
- Kyverno cleanup controller
- Kyverno reports controller
- ClusterPolicy resources
- Policy report generation

## Deployment model

The upstream Kyverno Helm chart was rendered locally and committed into Git.

The rendered manifest intentionally excludes Helm hooks and chart test resources.

Reason:

- Keep Kyverno GitOps-managed and reproducible.
- Avoid one-shot Helm hook Jobs remaining as long-lived GitOps resources.
- Avoid ArgoCD OutOfSync drift from completed migration Jobs.
- Keep the deployment consistent with the rendered-manifest pattern used for the rest of the platform stack.

## Baseline policies

The initial policy set is configured in `Audit` mode.

Policies:

- `p2-baseline-require-common-labels`
- `p2-baseline-disallow-privileged-apps`
- `p2-baseline-require-resource-requests-apps`

Scope:

- Namespace: `apps`

## Why Audit mode

The platform contains system components such as Cilium, Longhorn, ingress-nginx, ArgoCD, and monitoring controllers. Starting with `Audit` mode avoids breaking platform workloads while still producing policy reports.

After the baseline is understood and exceptions are documented, selected policies can be moved from `Audit` to `Enforce`.

## Smoke test

The smoke test created intentionally non-compliant resources in the `apps` namespace:

- A Deployment missing the `app.kubernetes.io/name` label.
- A Pod missing CPU and memory requests.
- A Pod using a privileged container.

Because policies are in `Audit` mode, the resources were allowed, but Kyverno reported the violations.

## Evidence files

- `raw/argocd-applications.txt`
- `raw/kyverno-resources.txt`
- `raw/clusterpolicies.txt`
- `raw/clusterpolicies.yaml`
- `raw/smoke-resources.txt`
- `raw/policyreports.yaml`
- `raw/admissionreports.yaml`
- `raw/non-running-pods.txt`
