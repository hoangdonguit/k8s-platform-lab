# Phase 1 - ingress-nginx LoadBalancer Evidence

## Goal

Expose ingress-nginx through a MetalLB-backed LoadBalancer instead of only using NodePort.

## Result

PASS.

- ingress-nginx controller Service type: `LoadBalancer`.
- Assigned LoadBalancer IP: `192.168.0.241`.
- MetalLB pool: `192.168.0.240-192.168.0.242`.
- Smoke Ingress host: `p2.local`.
- Smoke backend: `nginx:alpine`, 3 replicas.
- Curl from all 3 Kubernetes nodes to `http://192.168.0.241/` with `Host: p2.local` returned the nginx welcome page.
- ArgoCD applications were `Synced` / `Healthy`.

## Troubleshooting note

The first LoadBalancer attempt kept the Service in `<pending>`.

Root cause:

~~~text
MetalLB rejected the Service because it had both:
- metadata.annotations.metallb.io/loadBalancerIPs
- spec.loadBalancerIP
~~~

MetalLB controller log showed:

~~~text
service can not have both metallb.io/loadBalancerIPs and svc.Spec.LoadBalancerIP
~~~

The Git manifest was fixed to use only the MetalLB annotation:

~~~yaml
metadata:
  annotations:
    metallb.io/loadBalancerIPs: 192.168.0.241

spec:
  type: LoadBalancer
~~~

However, the live Service still retained the stale `spec.loadBalancerIP` field from the previous apply. A one-time patch was applied to remove it:

~~~bash
kubectl -n ingress-nginx patch svc ingress-nginx-controller \
  --type=json \
  -p='[{"op":"remove","path":"/spec/loadBalancerIP"}]'
~~~

After that, MetalLB assigned `192.168.0.241` and ArgoCD returned to `Healthy`.

## Final validation

The following checks passed:

- `p2-ingress-nginx`: `Synced` / `Healthy`
- `ingress-nginx-controller`: `LoadBalancer`, external IP `192.168.0.241`
- Smoke Ingress `p2.local`: address `192.168.0.241`
- Smoke Deployment: `3/3` available
- Curl from:
  - `p2-node-1`
  - `p2-node-2`
  - `p2-node-3`

All returned the nginx welcome page.

## Evidence files

- `raw/argocd-applications.txt`
- `raw/ingress-nginx-resources.txt`
- `raw/ingress-nginx-controller-service.yaml`
- `raw/smoke-resources.txt`
- `raw/smoke-ingress.yaml`
- `raw/metallb-pool.txt`
- `raw/metallb-controller-ingress-lb.log`
- `raw/curl-from-p2-node-1.txt`
- `raw/curl-from-p2-node-2.txt`
- `raw/curl-from-p2-node-3.txt`
- `raw/non-running-pods.txt`
