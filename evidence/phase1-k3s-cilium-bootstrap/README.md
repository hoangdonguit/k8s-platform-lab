# Phase 1 - K3s HA + Cilium Bootstrap Evidence

Date: 2026-07-03T01:31:26+07:00

## Summary

- Bootstrapped 3-node K3s cluster for Project 2: k8s-platform-lab.
- Disabled default flannel/traefik/servicelb during K3s install.
- Installed Cilium as the primary CNI.
- Verified all 3 nodes are Ready.
- Verified Cilium DaemonSet is 3/3 Ready.
- Verified CoreDNS, metrics-server and local-path-provisioner are Running.
- Verified multi-node workload scheduling with nginx replicas across all 3 nodes.
- Verified in-cluster DNS and ClusterIP service routing using curl to nginx.p2-smoke.svc.cluster.local.

## Current limitation

The current Kubernetes API access path uses p2-node-1 as the endpoint. This is acceptable for the MVP because floating IP/VPN is not ready yet. Tailscale remains the admin path.
