# Phase 1 Pre-Rebuild Evidence

## Context

The three OpenStack instances were accessible through Tailscale because OpenVPN connected successfully but traffic to the OpenStack floating IP range still timed out even after adding the 192.168.120.0/23 route through the VPN gateway.

## Current access path

| Node | Tailscale IP | Private IP |
|---|---:|---:|
| node-1 | 100.124.205.88 | 192.168.0.11 |
| node-2 | 100.115.253.101 | 192.168.0.12 |
| node-3 | 100.95.145.92 | 192.168.0.13 |

## Old cluster finding

The existing cluster is a default K3s cluster:

- node-1 is the only control-plane node.
- node-2 and node-3 are worker nodes.
- Flannel is enabled.
- Traefik and ServiceLB are enabled.
- local-path is the default StorageClass.
- No application workload, PV, or PVC was observed during the audit.

## Target rebuild

The old cluster will be rebuilt into the k8s-platform-lab target architecture:

- 3 K3s server/control-plane nodes
- embedded etcd HA
- Flannel disabled
- K3s network-policy controller disabled
- Traefik disabled
- ServiceLB disabled
- Cilium CNI installed after K3s bootstrap
- Tailscale kept only as temporary management access

## Safety note

Private backup archives are stored under private-backups/ and must not be committed to Git.
