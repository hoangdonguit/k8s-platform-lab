# MetalLB

This directory contains rendered MetalLB manifests generated from the upstream Helm chart.

Reason for rendered-manifest mode:

- ArgoCD repo-server previously had unstable access to external Helm repositories.
- The chart is rendered locally and committed to Git.
- ArgoCD applies manifests from GitHub.

Chart:

- Repository: https://metallb.github.io/metallb
- Chart: metallb
- Version: 0.16.1

Current mode:

- Base installation only.
- IPAddressPool and L2Advertisement are added in a separate step after CRDs/controllers are healthy.

Notes:

- The cluster runs on an OpenStack private network.
- MetalLB L2 mode may require OpenStack port security / allowed-address-pairs configuration.
