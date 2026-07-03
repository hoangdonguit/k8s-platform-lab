# Phase 1 - cert-manager Self-Signed Evidence

Date: 2026-07-03T17:26:11+07:00

## Summary

- Installed cert-manager through ArgoCD App-of-Apps.
- Used rendered-manifest GitOps mode instead of external Helm source because ArgoCD repo-server previously had unstable access to external Helm repositories.
- Verified p2-cert-manager is Synced and Healthy.
- Verified cert-manager, cert-manager-cainjector and cert-manager-webhook are Running.
- Verified cert-manager CRDs are installed.
- Created selfsigned-cluster-issuer as a lab bootstrap ClusterIssuer.
- Verified selfsigned-cluster-issuer is Ready=True.
- Created a smoke Certificate for p2.local in the apps namespace.
- Verified cert-manager issued a kubernetes.io/tls Secret.

## Limitation

- This is a self-signed lab issuer, not a public trusted CA.
- Let's Encrypt/ACME will be added later after domain, DNS and ingress exposure strategy are decided.
