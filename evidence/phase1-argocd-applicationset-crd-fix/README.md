# Phase 1 - ArgoCD ApplicationSet CRD Fix Evidence

Date: 2026-07-03T22:28:23+07:00

## Summary

- Detected argocd-applicationset-controller in CrashLoopBackOff.
- Root cause was a missing ApplicationSet CRD: applicationsets.argoproj.io.
- Added the missing ApplicationSet CRD through a GitOps-managed ArgoCD application: p2-argocd-crds.
- Used Server-Side Apply for the CRD because ArgoCD ApplicationSet CRDs are large.
- Verified p2-argocd-crds is Synced and Healthy.
- Verified applicationsets.argoproj.io exists.
- Restarted argocd-applicationset-controller after the CRD appeared.
- Verified the new ApplicationSet controller pod is Running with 0 restarts.
- Verified there are no non-running pods across the cluster.

## Why this matters

The ArgoCD ApplicationSet controller requires the ApplicationSet CRD to start correctly. Without the CRD, the controller cannot create its informer/cache for ApplicationSet resources and exits repeatedly.
