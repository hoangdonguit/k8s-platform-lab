# Application Onboarding - GitOps CI/CD Demo App

## Purpose

This document explains how the `gitops-cicd-k3s-lab` application is onboarded onto the `k8s-platform-lab` Kubernetes platform.

The two repositories remain separate projects:

- `gitops-cicd-k3s-lab`: application source code, Dockerfile, CI/CD workflow, and Kubernetes app manifests.
- `k8s-platform-lab`: Kubernetes platform, GitOps control plane, ingress, observability, storage, and policy baseline.

This onboarding proves that the platform can run a real application from an external CI/CD repository using ArgoCD GitOps.

## Repository roles

### Project 1: gitops-cicd-k3s-lab

Responsibilities:

- Flask demo application
- Unit tests
- Docker image build
- Docker Hub image push
- Kustomize application manifests
- GitHub Actions CI/CD pipeline
- Image tag update in Git

Current image:

- `hoangdonguit/gitops-cicd-demo-app:sha-c73671f`

Project 1 added a platform-specific overlay:

- `k8s/overlays/p2-platform`

This overlay deploys the app into the `apps` namespace and exposes it through an Ingress host:

- `demo.p2.local`

### Project 2: k8s-platform-lab

Responsibilities:

- 3-node K3s platform
- Cilium CNI
- ArgoCD App-of-Apps
- ingress-nginx
- MetalLB
- cert-manager
- kube-prometheus-stack
- Longhorn
- Kyverno policy baseline

Project 2 owns the ArgoCD Application:

- `p2-portfolio-gitops-cicd-demo`

The ArgoCD Application points to Project 1:

- Repository: `https://github.com/hoangdonguit/gitops-cicd-k3s-lab.git`
- Path: `k8s/overlays/p2-platform`
- Target revision: `main`
- Destination namespace: `apps`

## Traffic flow

Request flow:

1. Client sends HTTP request with host `demo.p2.local`.
2. MetalLB exposes ingress-nginx using LoadBalancer IP `192.168.0.241`.
3. ingress-nginx receives the request.
4. Ingress routes traffic to Service `gitops-cicd-demo-app`.
5. Service forwards traffic to the application Pod on port `8080`.

Runtime objects:

- Deployment: `apps/gitops-cicd-demo-app`
- Service: `apps/gitops-cicd-demo-app`
- Ingress: `apps/gitops-cicd-demo-app`
- Host: `demo.p2.local`
- LoadBalancer IP: `192.168.0.241`

## Validation result

The app was successfully deployed through ArgoCD.

Observed result:

- ArgoCD Application: `Synced` / `Healthy`
- Deployment: `1/1` available
- Pod: `Running`
- Service endpoint: `10.42.2.89:8080`
- Ingress address: `192.168.0.241`
- HTTP `/`: `200 OK`
- HTTP `/healthz`: `200 OK`

HTTP was validated from all three cluster nodes because the MetalLB IP is on the cluster private network.

## Local access note

Direct curl from the admin machine to `192.168.0.241` may time out if the admin machine cannot route to the cluster private network.

This is not an application failure.

For this lab, valid tests are:

- curl from cluster nodes to `192.168.0.241`
- curl from inside the cluster to the Service
- kubectl port-forward for local-only access

## Policy validation

Kyverno policy reports show that the onboarded app passes the current baseline policies.

Current result:

- Deployment: fail `0`
- ReplicaSet: fail `0`
- Pod: fail `0`

Validated policies include:

- `p2-baseline-require-common-labels`
- `p2-baseline-disallow-privileged-apps`
- `p2-baseline-require-resource-requests-apps`

This confirms the application has required labels, does not run privileged containers, and defines CPU/memory requests.

## Why this matters

This checkpoint connects the two portfolio projects.

Project 1 proves application CI/CD:

- test
- build
- push image
- update manifest
- GitOps deployment readiness

Project 2 proves platform engineering:

- platform bootstrap
- GitOps app onboarding
- ingress exposure
- policy validation
- operational evidence

Together, they demonstrate an end-to-end DevOps and Platform Engineering workflow.
