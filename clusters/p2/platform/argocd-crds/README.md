# ArgoCD CRDs

This directory contains ArgoCD CRDs that are required by the installed ArgoCD controllers.

## Why this exists

The ArgoCD ApplicationSet controller was installed, but the ApplicationSet CRD was missing.
As a result, the controller failed with:

```
failed to get restmapping: no matches for kind "ApplicationSet" in version "argoproj.io/v1alpha1"
```

This directory adds the missing `applicationsets.argoproj.io` CRD through GitOps.

## Version

- ArgoCD version: v3.4.4
- CRD source: upstream ArgoCD manifests/crds/applicationset-crd.yaml

## Apply mode

ApplicationSet CRD is large in ArgoCD 3.x, so this application must use ServerSideApply.
