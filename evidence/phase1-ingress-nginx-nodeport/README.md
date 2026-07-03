# Phase 1 - ingress-nginx NodePort Evidence

Date: 2026-07-03T15:08:39+07:00

## Summary

- Installed ingress-nginx through ArgoCD App-of-Apps.
- Switched ingress-nginx from external Helm source to rendered manifests committed in Git because ArgoCD repo-server could not reliably fetch the external Helm repository.
- Verified p2-ingress-nginx is Synced and Healthy.
- Verified ingress-nginx controller runs as a DaemonSet with 3/3 pods Ready.
- Verified NodePort service exposes HTTP 30080 and HTTPS 30443.
- Verified IngressClass nginx exists and is the default ingress class.
- Deployed a temporary nginx smoke workload in the apps namespace.
- Verified Ingress rule host p2.local routes to apps/ingress-smoke-nginx.
- Verified HTTP routing through ingress-nginx using Host header p2.local.
- Tested NodePort access through all three Tailscale node IPs.

## Troubleshooting notes

- The first smoke curl returned 404 because the Ingress had just been created and ingress-nginx had not reloaded yet.
- After controller reload, nginx config contained server_name p2.local and curl returned HTTP 200.
- One ingress controller pod initially stayed in ContainerCreating because image pull on p2-node-1 was slow. It later completed without manual image copy.

## Current limitation

- MetalLB is not installed yet.
- NodePort is used deliberately as the first ingress access path because LoadBalancer IP behavior on the OpenStack private network has not been verified yet.
