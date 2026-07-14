# Phase 1 - Platform MVP Final State

## Goal

Record the final clean state of the Kubernetes platform lab MVP.

This checkpoint proves that the core platform components are deployed through GitOps, healthy, and ready for application onboarding.

## Result

PASS.

All ArgoCD applications reached `Synced` / `Healthy`.

## Platform stack

### Kubernetes base

- 3-node K3s cluster
- All nodes are `Ready`
- Cilium CNI

### GitOps

- ArgoCD
- App-of-Apps model
- Git repository as source of truth
- Rendered Helm manifests committed into Git for reproducibility

### Networking and ingress

- ingress-nginx
- MetalLB L2 LoadBalancer
- ingress-nginx exposed with LoadBalancer IP `192.168.0.241`

### TLS automation

- cert-manager
- Self-signed ClusterIssuer baseline

### Observability

- kube-prometheus-stack
- Prometheus
- Alertmanager
- Grafana
- kube-state-metrics
- node-exporter
- ServiceMonitor and PrometheusRule CRDs

### Storage

- Longhorn
- CSI-based dynamic provisioning
- Longhorn installed as a non-default StorageClass
- `local-path` remains the default StorageClass
- Longhorn PVC smoke test passed before cleanup

### Policy and security baseline

- Kyverno
- Policy-as-code baseline
- Initial policies run in `Audit` mode
- PolicyReport smoke test passed before cleanup

## Important design choices

### Rendered Helm manifests

The platform uses rendered Helm manifests committed into Git instead of letting ArgoCD fetch all external Helm charts directly.

Reason:

- Reproducible deployment
- Easier evidence review
- Less runtime dependency on external Helm repositories
- Consistent GitOps behavior across components

### Audit-first policy model

Kyverno policies start in `Audit` mode.

Reason:

- Avoid breaking system components during platform bootstrap
- Collect policy reports first
- Move selected policies to `Enforce` later after exceptions and workload standards are documented

### Non-default Longhorn StorageClass

Longhorn is not set as the default StorageClass.

Reason:

- Keep K3s `local-path` default behavior intact
- Require stateful workloads to explicitly opt in to Longhorn
- Reduce accidental persistent volume provisioning

## Clean state

At the end of this checkpoint:

- `apps` namespace has no temporary smoke resources
- No PV/PVC smoke resources remain
- No Longhorn smoke volume remains
- ArgoCD applications are clean
- No non-running pods are present, excluding command header output

## Evidence files

- `raw/git-state.txt`
- `raw/argocd-applications.txt`
- `raw/nodes.txt`
- `raw/namespaces.txt`
- `raw/non-running-pods.txt`
- `raw/ingress-nginx.txt`
- `raw/metallb.txt`
- `raw/cert-manager.txt`
- `raw/clusterissuers.txt`
- `raw/monitoring.txt`
- `raw/servicemonitors.txt`
- `raw/prometheusrules.txt`
- `raw/longhorn.txt`
- `raw/storageclasses.txt`
- `raw/longhorn-volumes.txt`
- `raw/kyverno.txt`
- `raw/clusterpolicies.txt`
- `raw/policyreports.txt`
- `raw/apps-namespace.txt`
