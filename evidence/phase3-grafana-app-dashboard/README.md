# Phase 3 - Grafana App Dashboard Evidence

## Goal

Verify that the platform provides a Grafana dashboard for the onboarded GitOps CI/CD demo app.

## Result

PASS.

The dashboard is managed as code through a Kubernetes ConfigMap with label `grafana_dashboard=1`, loaded by the Grafana sidecar, and backed by Prometheus queries with real application metrics.

## Dashboard

- Title: `GitOps CI/CD Demo App`
- UID: `gitops-cicd-demo-app`
- Namespace: `monitoring`
- ConfigMap: `gitops-cicd-demo-app-dashboard`
- Source path: `platform/grafana-app-dashboards`

## Panels

The dashboard includes:

- App version
- Scrape target status
- Ready replicas
- Current firing app alerts
- Request rate by route
- P95 latency by route
- 5xx error ratio
- Container CPU usage
- Container memory working set
- Firing app alerts over time

## Why this matters

This checkpoint completes the observability workflow:

- custom app metrics
- ServiceMonitor scraping
- Prometheus alert rules
- alert fire-drill
- Grafana dashboard visualization

This makes the platform easier to demonstrate in a CV, interview, or project review.

## Evidence files

- `raw/project-dashboard-git-state.txt`
- `raw/argocd-applications.txt`
- `raw/grafana-dashboard-configmap.yaml`
- `raw/grafana-dashboard-sidecar-logs.txt`
- `raw/grafana-runtime.txt`
- `raw/query-app-version.json`
- `raw/query-scrape-up.json`
- `raw/query-ready-replicas.json`
- `raw/query-request-rate-by-route.json`
- `raw/query-p95-latency-by-route.json`
- `raw/query-5xx-ratio.json`
- `raw/query-firing-alerts.json`
- `screenshots/grafana-app-dashboard.png`
