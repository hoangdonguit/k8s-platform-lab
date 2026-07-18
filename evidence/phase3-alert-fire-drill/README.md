# Phase 3 - Alert Fire-drill Evidence

## Goal

Verify that the Prometheus alert pipeline can move an application alert from inactive to firing without breaking the real application.

## Method

A temporary GitOps-managed PrometheusRule was added for fire-drill validation.

The temporary alert used:

- Alert: `GitOpsDemoAppFireDrill`
- Expr: `vector(1)`
- For: `30s`
- Severity: `info`

This intentionally fires without changing the real application runtime.

After the firing state was confirmed, the temporary rule was removed through GitOps.

## Result

PASS.

## Validated flow

1. Temporary fire-drill rule added to Project 1 overlay.
2. ArgoCD synced the rule into Project 2.
3. Prometheus loaded the fire-drill rule group.
4. Alert `GitOpsDemoAppFireDrill` reached `firing`.
5. Temporary rule was removed through GitOps.
6. Prometheus no longer showed the fire-drill rule or alert.
7. Baseline app alert rules remained loaded.

## Why this matters

This proves that the platform alerting pipeline works end-to-end.

The platform does not only define PrometheusRule objects. It can also evaluate and fire alerts.

This is useful for CV and interview discussion because it demonstrates SRE-style validation:

- alert rule delivery through GitOps
- Prometheus rule loading
- firing-state verification
- controlled cleanup and recovery

## Evidence files

- `raw/project1-fire-drill-added-state.txt`
- `raw/argocd-fire-drill-revision.txt`
- `raw/apps-prometheusrules-before-cleanup.txt`
- `raw/fire-drill-prometheusrule.yaml`
- `raw/app-runtime-before-cleanup.txt`
- `raw/prometheus-loaded-fire-drill-rule-firing.json`
- `raw/prometheus-fire-drill-alert-firing.json`
- `raw/prometheus-fire-drill-rule-after-cleanup.json`
- `raw/prometheus-fire-drill-alert-after-cleanup.json`
- `raw/prometheus-baseline-app-alert-rules-after-cleanup.json`
