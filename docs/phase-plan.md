# Phase Plan

## Phase 1 - Repo, topology, and K3s HA bootstrap

Goal:
- Prepare repository structure.
- Define topology and node inventory.
- Bootstrap a 3-server-node K3s HA cluster with embedded etcd.

Deliverables:
- docs/topology.md
- docs/bootstrap.md
- evidence/phase1-cluster-ready.md

## Phase 2 - Cilium CNI

Goal:
- Install Cilium as the cluster CNI.
- Validate pod networking and service connectivity.

MVP decision:
- Keep kube-proxy enabled in the first version.
- Do not enable kube-proxy replacement until the baseline cluster is stable.

Deliverables:
- docs/cilium.md
- evidence/phase2-cilium-ready.md

## Phase 3 - ArgoCD App-of-Apps

Goal:
- Install ArgoCD.
- Manage platform components declaratively through App-of-Apps.

Deliverables:
- docs/gitops-model.md
- platform/root-app/
- evidence/phase3-argocd-ready.md

## Phase 4 - LoadBalancer, Ingress, and TLS

Goal:
- Install MetalLB in L2 mode.
- Install ingress-nginx.
- Install cert-manager with self-signed or local CA issuer.
- Validate HTTPS ingress for a sample workload.

Deliverables:
- docs/ingress-tls.md
- evidence/phase4-ingress-tls-ready.md

## Phase 5 - Observability

Goal:
- Install kube-prometheus-stack.
- Validate Prometheus targets and Grafana access.

Deliverables:
- docs/observability.md
- evidence/phase5-observability-ready.md

## Phase 6 - Storage

Goal:
- Install Longhorn.
- Create a PVC-backed test workload.
- Prove data survives pod restart.

Deliverables:
- docs/storage.md
- evidence/phase6-longhorn-pvc-test.md

## Phase 7 - Policy-as-code

Goal:
- Install Kyverno.
- Add basic security policies.

Policies:
- Block root containers.
- Require resource requests and limits.
- Block images using the latest tag.

Deliverables:
- docs/policy-as-code.md
- evidence/phase7-kyverno-policy-test.md

## Phase 8 - Tenant workload onboarding

Goal:
- Deploy gitops-cicd-k3s-lab as a tenant workload.
- Attach ingress, TLS, monitoring, and policy controls.

Deliverables:
- docs/app-onboarding.md
- evidence/phase8-sample-workload-ready.md
