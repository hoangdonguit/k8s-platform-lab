# Phase 3 - App Alerting Baseline Evidence

## Goal

Verify that the onboarded GitOps CI/CD demo app has Prometheus alert rules loaded and evaluated by the platform monitoring stack.

## Result

PASS.

The PrometheusRule was created through GitOps, validated by Prometheus Operator, loaded by Prometheus, and evaluated as inactive while the app is healthy.

## Application

- Name: `gitops-cicd-demo-app`
- Namespace: `apps`
- Image: `hoangdonguit/gitops-cicd-demo-app:sha-b10ebf1`
- Metrics path: `/metrics`
- ServiceMonitor: `apps/gitops-cicd-demo-app`
- PrometheusRule: `apps/gitops-cicd-demo-app-alerts`

## Alert rules

The app alert group is:

- `gitops-cicd-demo-app.rules`

Rules:

- `GitOpsDemoAppTargetDown`
- `GitOpsDemoAppNoReadyPods`
- `GitOpsDemoAppHigh5xxRate`
- `GitOpsDemoAppHighP95Latency`

## Expected healthy state

When the app is healthy:

- PrometheusRule exists.
- Prometheus Operator adds `prometheus-operator-validated: true`.
- Prometheus loads the rule group.
- Each rule state is `inactive`.
- Current app alerts are empty.

This is the expected result because the app is running, scrape target is up, no 5xx ratio problem is present, and p95 latency is below the threshold.

## Why this matters

This checkpoint moves the platform from observability to alerting.

The platform can now detect:

- application scrape target down
- no ready app pods
- high HTTP 5xx ratio
- high p95 latency

This provides an SRE-style baseline for operating the onboarded workload.

## Evidence files

- `raw/project1-alert-rules-state.txt`
- `raw/argocd-applications.txt`
- `raw/app-runtime-with-alert-rules.txt`
- `raw/app-prometheusrule.yaml`
- `raw/app-prometheusrule-describe.txt`
- `raw/non-running-pods.txt`
- `raw/prometheus-ready.txt`
- `raw/prometheus-loaded-app-alert-rules.json`
- `raw/prometheus-current-app-alerts.json`
- `raw/prometheus-app-alert-rule-names.txt`
