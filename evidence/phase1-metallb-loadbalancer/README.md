# Phase 1 - MetalLB LoadBalancer Evidence

Date: 2026-07-03T22:50:18+07:00

## Summary

- Installed MetalLB through ArgoCD App-of-Apps.
- Used rendered-manifest GitOps mode.
- Verified p2-metallb is Synced and Healthy.
- Verified metallb-controller is Running.
- Verified metallb-speaker runs as a DaemonSet with 3/3 pods Ready.
- Added IPAddressPool p2-private-pool with range 192.168.0.240-192.168.0.242.
- Added L2Advertisement p2-l2-advertisement.
- Created a temporary nginx LoadBalancer smoke service.
- Verified MetalLB assigned EXTERNAL-IP: 192.168.0.240.
- Verified HTTP access to 192.168.0.240 from all three cluster nodes.

## Result

MetalLB L2 mode is usable inside the OpenStack private network for this cluster.

## Scope

- This test proves LoadBalancer access from nodes on the 192.168.0.0/24 private network.
- External client access still depends on routing/VPN/security-group reachability to the private network.
