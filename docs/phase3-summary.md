# Phase 3 Summary - App Alerting Baseline

## Purpose

Phase 3 adds an alerting baseline for the onboarded `gitops-cicd-demo-app`.

Phase 2 proved that the platform can deploy and observe the application. Phase 3 extends that by proving that the platform can evaluate alert rules for the application using Prometheus.

## Final status

Phase 3 baseline is complete.

The platform now has application-specific alert rules for:

- app metrics target down
- no ready app pods
- high HTTP 5xx error ratio
- high p95 latency

## Related repositories

### Project 1 - gitops-cicd-k3s-lab

Project 1 owns the application manifests and alert rule definition.

Important path:

- `k8s/overlays/p2-platform/prometheusrule.yaml`

Latest relevant Project 1 commit:

- `61a9713 feat: add app prometheus alert rules`

### Project 2 - k8s-platform-lab

Project 2 owns the platform and evidence.

Important ArgoCD application:

- `p2-portfolio-gitops-cicd-demo`

Latest Phase 3 evidence commit:

- `33ac7ae docs: record app alerting baseline evidence`

Documentation commit:

- `b916aef docs: add app alerting guide`

## Runtime state

Application:

- Name: `gitops-cicd-demo-app`
- Namespace: `apps`
- Image: `hoangdonguit/gitops-cicd-demo-app:sha-b10ebf1`
- ServiceMonitor: `apps/gitops-cicd-demo-app`
- PrometheusRule: `apps/gitops-cicd-demo-app-alerts`

ArgoCD:

- Application: `p2-portfolio-gitops-cicd-demo`
- Status: `Synced` / `Healthy`
- Revision: Project 1 commit containing the PrometheusRule

## Alert rules

Rule group:

- `gitops-cicd-demo-app.rules`

Rules:

### GitOpsDemoAppTargetDown

Detects when Prometheus cannot scrape the application metrics endpoint.

Signal:

- `up{job="gitops-cicd-demo-app",namespace="apps"} == 0`

Severity:

- `critical`

### GitOpsDemoAppNoReadyPods

Detects when the Deployment has no available replicas.

Signal:

- `kube_deployment_status_replicas_available{namespace="apps",deployment="gitops-cicd-demo-app"} < 1`

Severity:

- `critical`

### GitOpsDemoAppHigh5xxRate

Detects when the HTTP 5xx error ratio is higher than 5 percent.

Signal:

- custom app metric `gitops_cicd_demo_http_requests_total`

Severity:

- `warning`

### GitOpsDemoAppHighP95Latency

Detects when route-level p95 latency is higher than 500ms.

Signal:

- custom app histogram `gitops_cicd_demo_http_request_duration_seconds_bucket`

Severity:

- `warning`

## Validation result

The PrometheusRule was successfully:

- created through GitOps
- validated by Prometheus Operator
- loaded into Prometheus rule files
- evaluated by Prometheus

Expected healthy state:

- rule group exists
- all four rules are present
- all rules are `inactive`
- current app alerts list is empty

This is the correct result because the application is currently healthy.

## Why this matters

This phase moves the platform from observability to alerting.

Before Phase 3, the platform could show metrics.

After Phase 3, the platform can detect unhealthy conditions and produce alert signals.

This is closer to a real platform/SRE workflow:

- deploy app
- scrape metrics
- define app-level alert rules
- validate rule loading
- document first-response debugging

## Evidence

Evidence for this phase:

- `evidence/phase3-app-alerting-baseline`

Related docs:

- `docs/app-alerting.md`

## Alert fire-drill result

A controlled alert fire-drill was completed after the baseline alert rules were created.

The fire-drill used a temporary GitOps-managed PrometheusRule:

- Alert: `GitOpsDemoAppFireDrill`
- Expr: `vector(1)`
- For: `30s`
- Severity: `info`

Validated flow:

1. Temporary fire-drill rule was added through Project 1.
2. ArgoCD synced the rule into Project 2.
3. Prometheus loaded the fire-drill rule.
4. Alert state changed to `firing`.
5. Temporary rule was removed through GitOps.
6. Prometheus no longer showed the fire-drill rule or alert.
7. Baseline app alert rules remained loaded.

Evidence:

- `evidence/phase3-alert-fire-drill`

## Recommended next steps

Recommended next phase options:

1. Alert fire-drill
   - Safely prove an alert can move from inactive to firing.
   - Record evidence.
   - Revert to healthy state.

2. Grafana dashboard
   - Create app-level dashboard queries for request rate, p95 latency, 5xx ratio, and pod readiness.
   - Document dashboard access.

3. App autoscaling baseline
   - Add HPA using CPU or custom metrics later.
   - Run controlled load test.
   - Record scaling evidence.

4. Incident runbook
   - Add a runbook for each alert.
   - Include exact diagnosis commands and recovery steps.

