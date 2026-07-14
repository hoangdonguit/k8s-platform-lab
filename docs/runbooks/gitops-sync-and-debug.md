# Runbook - GitOps Sync and Debug

## Purpose

Check and troubleshoot ArgoCD GitOps applications.

## Check all applications

`kubectl -n argocd get applications.argoproj.io -o wide`

Compact view:

`kubectl -n argocd get applications.argoproj.io -o custom-columns=NAME:.metadata.name,SYNC:.status.sync.status,HEALTH:.status.health.status,REVISION:.status.sync.revision`

## Force refresh

Refresh root app:

`kubectl -n argocd annotate application p2-root-apps argocd.argoproj.io/refresh=hard --overwrite`

Refresh a child app:

`kubectl -n argocd annotate application <app-name> argocd.argoproj.io/refresh=hard --overwrite`

## Inspect an application

`kubectl -n argocd get app <app-name> -o yaml`

Show resources that are not synced:

`kubectl -n argocd get app <app-name> -o json | jq -r '.status.resources[] | select(.status != "Synced") | [(.group // ""), .kind, (.namespace // ""), .name, (.status // "-"), (.health.status // "-"), (.message // "-")] | @tsv'`

## Common causes

### OutOfSync

Possible causes:

- Live object was manually changed.
- Controller added runtime fields.
- Rendered Helm manifest changed.
- CRD defaulting changed live object.
- Hook or completed Job is still tracked by ArgoCD.

### Progressing

Possible causes:

- Deployment not available yet.
- Ingress has not received address yet.
- Pod image is still pulling.
- Readiness probe failing.

### Degraded

Possible causes:

- Pod CrashLoopBackOff.
- ImagePullBackOff.
- Service has no endpoints.
- Admission policy blocked resource.

## Recovery flow

1. Check ArgoCD app status.
2. Inspect app resources.
3. Check namespace objects.
4. Check pods and events.
5. Check controller logs if needed.
6. Fix Git manifest first.
7. Avoid manual cluster changes unless used for short-lived debugging.
