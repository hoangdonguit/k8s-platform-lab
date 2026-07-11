# ingress-nginx

This directory contains rendered ingress-nginx manifests generated from the upstream Helm chart.

Reason for rendered-manifest mode:

- ArgoCD repo-server previously had unstable access to external Helm repositories.
- The chart is rendered locally and committed to Git.
- ArgoCD applies manifests from GitHub.

Chart:

- Repository: https://kubernetes.github.io/ingress-nginx
- Chart: ingress-nginx
- Version: 4.15.1

Current mode:

- Controller runs as DaemonSet.
- IngressClass: nginx.
- Default IngressClass: true.
- Service type: LoadBalancer.
- MetalLB-assigned LoadBalancer IP: 192.168.0.241.

Notes:

- MetalLB pool: 192.168.0.240-192.168.0.242.
- This LoadBalancer IP is on the OpenStack private network.
- Direct access from the laptop depends on routing/VPN reachability to 192.168.0.0/24.
