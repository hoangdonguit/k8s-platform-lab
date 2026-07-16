# App Alerting Baseline

## Purpose

This document explains the alerting baseline for the onboarded `gitops-cicd-demo-app`.

The goal is to move beyond basic monitoring and provide an SRE-style signal layer for the application.

The platform should be able to detect:

- the app metrics target is down
- the app has no ready pods
- the app has a high HTTP 5xx error ratio
- the app has high p95 latency

## Application

- Name: `gitops-cicd-demo-app`
- Namespace: `apps`
- Metrics path: `/metrics`
- ServiceMonitor: `apps/gitops-cicd-demo-app`
- PrometheusRule: `apps/gitops-cicd-demo-app-alerts`
- Rule group: `gitops-cicd-demo-app.rules`

## Alert rules

### GitOpsDemoAppTargetDown

Purpose:

Detect when Prometheus cannot scrape the application metrics endpoint.

Query:

`up{job="gitops-cicd-demo-app",namespace="apps"} == 0`

Duration:

`2m`

Severity:

`critical`

What it means:

- The app may be down.
- The ServiceMonitor may be broken.
- The Service may have no endpoints.
- The `/metrics` endpoint may be failing.
- Network path from Prometheus to the app may be broken.

First checks:

`kubectl -n apps get pod,svc,endpointslice,servicemonitor -o wide`

`kubectl -n apps describe servicemonitor gitops-cicd-demo-app`

`kubectl -n apps run metrics-check --rm -i --restart=Never --image=curlimages/curl:8.10.1 -- curl -si http://gitops-cicd-demo-app/metrics`

### GitOpsDemoAppNoReadyPods

Purpose:

Detect when the Deployment has no available replicas.

Query:

`kube_deployment_status_replicas_available{namespace="apps",deployment="gitops-cicd-demo-app"} < 1`

Duration:

`2m`

Severity:

`critical`

What it means:

- The app Pod may be crashing.
- The image may fail to pull.
- The readiness probe may be failing.
- The scheduler may not be able to place the Pod.
- A policy or resource issue may prevent the workload from becoming ready.

First checks:

`kubectl -n apps get deploy,rs,pod -o wide`

`kubectl -n apps describe deploy gitops-cicd-demo-app`

`kubectl -n apps describe pod -l app=gitops-cicd-demo-app`

`kubectl -n apps logs deploy/gitops-cicd-demo-app --tail=120`

### GitOpsDemoAppHigh5xxRate

Purpose:

Detect a high ratio of HTTP 5xx responses from the app.

Query:

`(sum(rate(gitops_cicd_demo_http_requests_total{job="gitops-cicd-demo-app",namespace="apps",status=~"5.."}[5m])) / clamp_min(sum(rate(gitops_cicd_demo_http_requests_total{job="gitops-cicd-demo-app",namespace="apps"}[5m])), 0.001)) > 0.05`

Duration:

`5m`

Severity:

`warning`

What it means:

- More than 5 percent of requests are returning HTTP 5xx.
- The app may have internal errors.
- A dependency may be failing.
- A newly deployed image may contain a bug.

First checks:

`kubectl -n apps logs deploy/gitops-cicd-demo-app --tail=200`

`kubectl -n apps run app-check --rm -i --restart=Never --image=curlimages/curl:8.10.1 -- curl -si http://gitops-cicd-demo-app/`

Prometheus query:

`sum by (route,method,status) (rate(gitops_cicd_demo_http_requests_total{job="gitops-cicd-demo-app"}[5m]))`

### GitOpsDemoAppHighP95Latency

Purpose:

Detect high route-level p95 latency.

Query:

`histogram_quantile(0.95, sum by (le, route, method) (rate(gitops_cicd_demo_http_request_duration_seconds_bucket{job="gitops-cicd-demo-app",namespace="apps",route!=""}[5m]))) > 0.5`

Duration:

`5m`

Severity:

`warning`

What it means:

- One or more app routes have p95 latency above 500ms.
- The app may be overloaded.
- The node may be under resource pressure.
- The request path may be slow.
- A recent change may have introduced performance regression.

First checks:

`kubectl -n apps top pod`

`kubectl top node`

`kubectl -n apps describe pod -l app=gitops-cicd-demo-app`

Prometheus query:

`histogram_quantile(0.95, sum by (le, route, method) (rate(gitops_cicd_demo_http_request_duration_seconds_bucket{job="gitops-cicd-demo-app",route!=""}[5m])))`

## Healthy state

When the application is healthy:

- PrometheusRule exists.
- Prometheus Operator validates the rule.
- Prometheus loads the rule group.
- All app alert rules are `inactive`.
- The current app alerts list is empty.

Useful check:

`kubectl -n apps get prometheusrule gitops-cicd-demo-app-alerts -o yaml`

Prometheus API check:

`curl -s http://kube-prometheus-stack-prometheus.monitoring.svc:9090/api/v1/rules?type=alert`

## Why inactive is good

An alert rule being `inactive` means:

- Prometheus loaded the rule.
- Prometheus is evaluating the rule.
- The alert condition is currently false.

For the baseline evidence, `inactive` is the expected state because the app is healthy.

## Evidence

Evidence for this checkpoint is stored in:

- `evidence/phase3-app-alerting-baseline`

This evidence proves:

- the PrometheusRule exists
- the rule is validated by Prometheus Operator
- Prometheus loaded the rule group
- all four app alert rules exist
- all app alert rules are inactive in the healthy state
