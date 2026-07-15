# Phase 2 - Custom Application Metrics Evidence

## Goal

Verify end-to-end application metrics for the onboarded GitOps CI/CD demo app.

This checkpoint proves that:

- The Flask app exposes Prometheus metrics on `/metrics`.
- The app image was rebuilt and redeployed through the CI/CD + GitOps flow.
- ArgoCD deployed the new image onto the platform.
- A ServiceMonitor was created for the app.
- Prometheus discovered and scraped the app target.
- Custom application metrics are queryable from Prometheus.

## Result

PASS.

## Application

- Name: `gitops-cicd-demo-app`
- Namespace: `apps`
- Image: `hoangdonguit/gitops-cicd-demo-app:sha-2218381`
- Ingress host: `demo.p2.local`
- Ingress LoadBalancer IP: `192.168.0.241`
- Metrics path: `/metrics`

## Custom metrics

The app exposes these custom metrics:

- `gitops_cicd_demo_app_info`
- `gitops_cicd_demo_http_requests_total`
- `gitops_cicd_demo_http_request_duration_seconds`

The app also exposes default Python/process metrics from `prometheus-client`.

## Prometheus integration

The app is scraped by Prometheus through a ServiceMonitor:

- ServiceMonitor: `apps/gitops-cicd-demo-app`
- Selected Service: `apps/gitops-cicd-demo-app`
- Endpoint port: `http`
- Path: `/metrics`

## Why this matters

The previous observability checkpoint proved that the platform could observe the workload using Kubernetes-level metrics from kube-state-metrics and kubelet/cAdvisor.

This checkpoint proves application-level observability.

Together, they show that the platform supports both:

- infrastructure/workload metrics
- application custom metrics

## Evidence files

- `raw/argocd-applications.txt`
- `raw/app-runtime-with-servicemonitor.txt`
- `raw/app-servicemonitor.yaml`
- `raw/app-deployment.yaml`
- `raw/non-running-pods.txt`
- `raw/prometheus-ready.txt`
- `raw/prometheus-active-target-app.json`
- `raw/query-gitops-app-info.json`
- `raw/query-gitops-http-requests-total.json`
- `raw/query-gitops-http-request-rate.json`
- `raw/query-gitops-http-duration-count.json`
- `raw/query-gitops-http-p95.json`
- `raw/ingress-metrics-test.txt`
