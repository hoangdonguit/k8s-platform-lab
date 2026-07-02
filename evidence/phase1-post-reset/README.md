# Phase 1 Post-Reset Evidence

## Goal

Verify that the old default K3s cluster was removed before rebuilding the target k8s-platform-lab cluster.

## Result

The old K3s cluster was removed from all three nodes.

Verified state:

- k3s service is inactive.
- k3s-agent service is inactive.
- Tailscale remains active for temporary management access.
- cni0 and flannel.1 are no longer present.
- K3s and CNI directories are no longer present.
- The nodes are ready for a clean K3s HA bootstrap.

## Nodes

| Node | Tailscale IP | Private IP |
|---|---:|---:|
| node-1 | 100.124.205.88 | 192.168.0.11 |
| node-2 | 100.115.253.101 | 192.168.0.12 |
| node-3 | 100.95.145.92 | 192.168.0.13 |
