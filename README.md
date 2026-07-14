# k8s-platform-lab

A Kubernetes Platform Engineering lab built on a 3-node K3s cluster.

This project demonstrates how to bootstrap, operate, and validate a small but realistic Kubernetes platform using GitOps, ingress, observability, storage, and policy-as-code.

## Purpose

This repository is Project 2 in the portfolio.

It focuses on platform engineering:

- Kubernetes cluster operations
- GitOps platform delivery
- Ingress and LoadBalancer setup
- Monitoring and observability
- Persistent storage
- Policy-as-code
- Application onboarding

A separate repository, `gitops-cicd-k3s-lab`, is Project 1. It focuses on application CI/CD. This platform onboards that application through ArgoCD to prove an end-to-end DevOps workflow.

## Current platform stack

| Layer | Tool | Purpose |
|---|---|---|
| Kubernetes | K3s | Lightweight 3-node Kubernetes cluster |
| CNI | Cilium | Pod networking and network policy foundation |
| GitOps | ArgoCD | App-of-Apps deployment model |
| Ingress | ingress-nginx | HTTP routing into the cluster |
| LoadBalancer | MetalLB | Bare-metal/private-network LoadBalancer IPs |
| TLS | cert-manager | Certificate automation baseline |
| Monitoring | kube-prometheus-stack | Prometheus, Alertmanager, Grafana, node-exporter, kube-state-metrics |
| Storage | Longhorn | CSI distributed block storage |
| Policy | Kyverno | Policy-as-code and audit reports |

## Cluster topology

| Node | Role | Internal IP |
|---|---|---|
| p2-node-1 | control-plane, etcd | 192.168.0.11 |
| p2-node-2 | control-plane, etcd | 192.168.0.12 |
| p2-node-3 | control-plane, etcd | 192.168.0.13 |

The cluster is administered through a kubeconfig file on the admin machine.

## GitOps model

The platform uses ArgoCD App-of-Apps.

Root application:

- `p2-root-apps`

Platform applications:

- `p2-platform-namespaces`
- `p2-argocd-crds`
- `p2-ingress-nginx`
- `p2-cert-manager`
- `p2-metallb`
- `p2-kube-prometheus-stack`
- `p2-longhorn`
- `p2-kyverno`
- `p2-portfolio-gitops-cicd-demo`

Most upstream Helm charts are rendered locally and committed into Git. This keeps the lab reproducible and reduces runtime dependency on external Helm repositories during ArgoCD reconciliation.

## Networking and ingress

MetalLB provides private LoadBalancer IPs.

Current ingress-nginx LoadBalancer IP:

- `192.168.0.241`

The onboarded demo application is exposed through:

- Host: `demo.p2.local`
- Ingress class: `nginx`
- LoadBalancer IP: `192.168.0.241`

Because the LoadBalancer IP is on the cluster private network, direct curl from the admin machine may fail if the admin machine cannot route to `192.168.0.0/24`. Valid tests are performed from cluster nodes or from inside the cluster.

## Application onboarding

The platform onboards Project 1:

- Repository: `gitops-cicd-k3s-lab`
- App: `gitops-cicd-demo-app`
- Image: `hoangdonguit/gitops-cicd-demo-app:sha-c73671f`
- Project 1 overlay: `k8s/overlays/p2-platform`
- Project 2 ArgoCD app: `p2-portfolio-gitops-cicd-demo`
- Namespace: `apps`

This proves the platform can run a real application from a separate CI/CD repository through GitOps.

More details:

- `docs/application-onboarding.md`

## Policy baseline

Kyverno is installed as the policy-as-code layer.

Current baseline policies:

- `p2-baseline-require-common-labels`
- `p2-baseline-disallow-privileged-apps`
- `p2-baseline-require-resource-requests-apps`

The baseline starts in `Audit` mode to avoid breaking platform workloads during bootstrap.

The onboarded demo app passes the current Kyverno baseline with `fail = 0`.

## Storage baseline

Longhorn is installed as a non-default StorageClass.

StorageClasses:

- `local-path` as default
- `longhorn`
- `longhorn-static`

A PVC smoke test verified that data persisted across pod recreation before cleanup.

## Evidence index

Major evidence checkpoints:

- `evidence/phase1-post-reset`
- `evidence/phase1-k3s-cilium-bootstrap`
- `evidence/phase1-argocd-app-of-apps`
- `evidence/phase1-ingress-nginx-nodeport`
- `evidence/phase1-cert-manager-selfsigned`
- `evidence/phase1-argocd-applicationset-crd-fix`
- `evidence/phase1-metallb-loadbalancer`
- `evidence/phase1-ingress-nginx-loadbalancer`
- `evidence/phase1-kube-prometheus-stack`
- `evidence/phase1-longhorn-pvc-smoke`
- `evidence/phase1-kyverno-policy-baseline`
- `evidence/phase1-platform-mvp-final-state`
- `evidence/phase2-portfolio-app-onboarding`

## Useful commands

Set kubeconfig:

`export KUBECONFIG="$HOME/.kube/p2-k8s-platform-lab-p2-node-1.yaml"`

Check ArgoCD apps:

`kubectl -n argocd get applications.argoproj.io -o wide`

Check all non-running pods:

`kubectl get pods -A -o wide | grep -Ev 'Running|Completed' || true`

Check app runtime:

`kubectl -n apps get deploy,svc,ingress,pod,endpoints,endpointslice -o wide`

Test app from a cluster node:

`curl -si -H 'Host: demo.p2.local' http://192.168.0.241/`

Port-forward Grafana:

`kubectl -n monitoring port-forward svc/kube-prometheus-stack-grafana 3000:80 --address 127.0.0.1`

Port-forward Longhorn UI:

`kubectl -n longhorn-system port-forward svc/longhorn-frontend 8080:80 --address 127.0.0.1`

## Roadmap

Next improvements:

- Improve repository documentation and runbooks
- Add dashboard access hardening
- Add more Kyverno policies and selected Enforce mode
- Add workload monitoring evidence for the onboarded app
- Add backup and restore testing with Longhorn or Velero
