# App Dashboard Guide

## Purpose

This document explains the Grafana dashboard for the onboarded `gitops-cicd-demo-app`.

The dashboard provides a visual operational view of the application after it has been onboarded onto the `k8s-platform-lab` platform.

## Dashboard

- Title: `GitOps CI/CD Demo App`
- UID: `gitops-cicd-demo-app`
- Namespace: `monitoring`
- ConfigMap: `gitops-cicd-demo-app-dashboard`
- Source path: `platform/grafana-app-dashboards`

The dashboard is managed as code using a Kubernetes ConfigMap labeled:

- `grafana_dashboard=1`

Grafana's dashboard sidecar watches this label and automatically loads the dashboard.

## Panels

### App Version

Shows the currently deployed application version from:

`gitops_cicd_demo_app_info`

This helps confirm which image version is currently running.

### Scrape Target Up

Shows whether Prometheus can scrape the app metrics endpoint.

Query source:

`up{job="gitops-cicd-demo-app",namespace="apps"}`

Expected healthy value:

`1`

### Ready Replicas

Shows available replicas for the app Deployment.

Query source:

`kube_deployment_status_replicas_available`

Expected healthy value:

`1`

### Current App Alerts

Shows the number of currently firing app alerts.

Expected healthy value:

`0`

### Request Rate by Route

Shows HTTP request rate grouped by route, method, and status.

This helps identify traffic patterns and status-code distribution.

### P95 Latency by Route

Shows p95 latency from the app histogram metric.

This helps identify slow routes and performance regressions.

### 5xx Error Ratio

Shows the ratio of HTTP 5xx responses.

Expected healthy value:

`0`

### Container CPU Usage

Shows CPU usage for the application container.

This helps identify CPU pressure or unusual usage changes.

### Container Memory Working Set

Shows memory working set for the application container.

This helps identify memory growth or unexpected memory pressure.

### Firing App Alerts

Shows current firing alert count over time.

Expected healthy value:

`0`

## Why this matters

The dashboard completes the app observability story:

- The app exposes custom metrics.
- Prometheus scrapes the metrics through ServiceMonitor.
- PrometheusRule evaluates alert rules.
- Grafana visualizes application health and performance.

This makes the platform easier to demonstrate in a CV, interview, or project review.

## Evidence

Dashboard evidence is stored in:

- `evidence/phase3-grafana-app-dashboard`

The evidence includes:

- dashboard ConfigMap
- ArgoCD status
- Grafana sidecar logs
- Prometheus query results
- dashboard screenshot
