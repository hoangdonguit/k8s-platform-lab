# k8s-platform-lab

A production-like Kubernetes Platform Engineering lab built on a multi-node K3s HA cluster.

## Goal

This project focuses on building and operating the Kubernetes platform layer, not only deploying a demo application.

Main areas:

- K3s HA cluster bootstrap with embedded etcd
- Cilium CNI and NetworkPolicy
- GitOps with ArgoCD App-of-Apps
- MetalLB, ingress-nginx, and cert-manager
- Observability with kube-prometheus-stack and Grafana
- Persistent storage with Longhorn
- Policy-as-code with Kyverno
- Tenant workload onboarding

## Owner

- GitHub: hoangdonguit
- Docker Hub: hoangdonguit

## Scope

This is a production-like lab, not a production-ready platform.

The project prioritizes clear evidence over tool count. Each phase must include documentation, commands, runtime output, and validation evidence.
