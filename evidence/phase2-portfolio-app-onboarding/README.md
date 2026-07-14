# Phase 2 - Portfolio App Onboarding Evidence

## Goal

Onboard the GitOps CI/CD demo application from the separate `gitops-cicd-k3s-lab` repository onto the `k8s-platform-lab` Kubernetes platform.

## Result

PASS.

The application was deployed through the platform ArgoCD instance and reached `Synced` / `Healthy`.

## Architecture

Repository roles:

- `gitops-cicd-k3s-lab`: application source code, Dockerfile, CI/CD pipeline, Kubernetes app manifests.
- `k8s-platform-lab`: Kubernetes platform, ArgoCD App-of-Apps, ingress, networking, monitoring, storage, and policy baseline.

Onboarding model:

- Project 1 keeps ownership of the application and its Kustomize overlay.
- Project 2 owns the platform-level ArgoCD Application that deploys Project 1.
- ArgoCD in Project 2 pulls the `k8s/overlays/p2-platform` path from Project 1.

## Runtime result

Application:

- Name: `gitops-cicd-demo-app`
- Namespace: `apps`
- Image: `hoangdonguit/gitops-cicd-demo-app:sha-c73671f`
- Ingress host: `demo.p2.local`
- Ingress LoadBalancer IP: `192.168.0.241`

Validation:

- Deployment reached `1/1` available.
- Pod reached `Running`.
- Service endpoint resolved to the application pod.
- Ingress returned HTTP 200 from all three cluster nodes.
- In-cluster service test returned HTTP 200 on `/healthz`.

## Important note about local access

Direct curl from the admin machine to `192.168.0.241` may time out if the admin machine cannot route to the cluster private network.

This is not an application failure.

The correct validation for this lab is to test from cluster nodes or from inside the cluster, where the MetalLB private LoadBalancer IP is reachable.

## Why this matters

This checkpoint proves that the platform is not only a collection of Kubernetes tools. It can onboard and operate a real application from a separate CI/CD repository using GitOps.

This connects the two portfolio projects:

- Project 1 proves application CI/CD.
- Project 2 proves platform engineering and workload onboarding.

## Evidence files

- `raw/project2-git-state.txt`
- `raw/project1-overlay-state.txt`
- `raw/argocd-applications.txt`
- `raw/app-runtime.txt`
- `raw/app-ingress-describe.txt`
- `raw/app-deployment-describe.txt`
- `raw/policyreports.txt`
- `raw/non-running-pods.txt`
- `raw/ingress-http-tests-from-nodes.txt`
- `raw/in-cluster-service-test.txt`
