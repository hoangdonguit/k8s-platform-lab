# Phase 2 - Custom Application Metrics Route Label Evidence

## Goal

Polish application metrics labels to avoid label collision with Prometheus target labels.

The previous implementation used `endpoint` as an application metric label. Prometheus also attaches a target label named `endpoint`, so the application label was shown as `exported_endpoint` in Prometheus query results.

This checkpoint changes the application metric label from `endpoint` to `route`.

## Result

PASS.

## Application

- Name: `gitops-cicd-demo-app`
- Namespace: `apps`
- Image: `hoangdonguit/gitops-cicd-demo-app:sha-b10ebf1`
- Version: `sha-b10ebf1`
- Metrics path: `/metrics`

## Validated metrics

The application now exposes route-based labels:

- `gitops_cicd_demo_http_requests_total{method="GET",route="healthz",status="200"}`
- `gitops_cicd_demo_http_request_duration_seconds_count{method="GET",route="healthz"}`

Prometheus can query metrics by route using:

- `sum by (route,method,status) (...)`
- `histogram_quantile(... sum by (le,route,method) (...))`

## Why this matters

This makes application metrics cleaner and easier to use in dashboards, alerts, and SLO-style queries.

## Evidence files

- `raw/project1-route-metrics-state.txt`
- `raw/argocd-applications.txt`
- `raw/app-runtime.txt`
- `raw/app-deployment.yaml`
- `raw/app-servicemonitor.yaml`
- `raw/non-running-pods.txt`
- `raw/prometheus-ready.txt`
- `raw/prometheus-active-target-app.json`
- `raw/query-app-info-by-version.json`
- `raw/query-http-requests-by-route.json`
- `raw/query-http-duration-count-by-route.json`
- `raw/query-http-p95-by-route.json`
- `raw/ingress-route-metrics-test.txt`
