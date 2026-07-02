# Topology

## Cluster identity

| Item | Value |
|---|---|
| Project | k8s-platform-lab |
| Cluster type | K3s HA embedded etcd |
| Node model | 3 server nodes |
| Environment | OpenStack VM lab |
| GitHub owner | hoangdonguit |
| Docker Hub owner | hoangdonguit |

## Node inventory

| Node | Role | IP | OS | CPU | RAM | Disk | Notes |
|---|---|---|---|---:|---:|---:|---|
| p2-k8s-cp-1 | server/control-plane/etcd | TBD | TBD | TBD | TBD | TBD | First bootstrap node |
| p2-k8s-cp-2 | server/control-plane/etcd | TBD | TBD | TBD | TBD | TBD | Join server |
| p2-k8s-cp-3 | server/control-plane/etcd | TBD | TBD | TBD | TBD | TBD | Join server |

## Network plan

| Purpose | CIDR/IP range | Notes |
|---|---|---|
| Node network | TBD | OpenStack private subnet |
| Pod CIDR | 10.42.0.0/16 | K3s default unless changed |
| Service CIDR | 10.43.0.0/16 | K3s default unless changed |
| MetalLB pool | TBD | Must be unused IPs in the node subnet and allowed by OpenStack networking |

## Bootstrap decisions

- Use K3s HA with embedded etcd.
- Use 3 server nodes for etcd quorum.
- Disable Flannel because Cilium will be used as CNI.
- Disable K3s built-in network policy controller because Cilium will enforce NetworkPolicy.
- Disable Traefik because ingress-nginx will be used.
- Disable ServiceLB because MetalLB will provide LoadBalancer IPs.
- Keep CoreDNS enabled.
- Keep kube-proxy enabled in the MVP.
- Bootstrap Cilium before ArgoCD because the cluster needs working networking before GitOps controllers can run reliably.

## Out of MVP

- Vault
- Velero
- Loki
- Tempo
- Hubble
- Istio
- Rook-Ceph
