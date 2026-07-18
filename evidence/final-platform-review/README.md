# Final Platform Review Evidence

## Goal

Record the final healthy state of the `k8s-platform-lab` project after platform bootstrap, application onboarding, observability, alerting, fire-drill, and dashboard work.

## Result

PASS.

This checkpoint verifies that the platform is in a CV-ready state.

## Validated areas

- K3s cluster nodes
- ArgoCD App-of-Apps
- ingress-nginx
- MetalLB
- cert-manager
- kube-prometheus-stack
- Longhorn
- Kyverno
- onboarded GitOps CI/CD demo app
- ServiceMonitor scraping
- PrometheusRule alerting
- Grafana dashboard as code
- Kyverno policy reports
- no non-running pods

## Application

- App: `gitops-cicd-demo-app`
- Namespace: `apps`
- Image: `hoangdonguit/gitops-cicd-demo-app:sha-b10ebf1`
- Ingress host: `demo.p2.local`
- LoadBalancer IP: `192.168.0.241`
- Metrics path: `/metrics`
- Dashboard: `GitOps CI/CD Demo App`

## Why this matters

This final review proves that the project is not only a set of manifests. It is an operational platform with:

- GitOps delivery
- workload onboarding
- ingress exposure
- policy validation
- persistent storage baseline
- custom application metrics
- alerting baseline
- alert fire-drill validation
- dashboard visualization
- evidence-driven documentation

## Evidence files

- `raw/project2-git-state.txt`
- `raw/project1-git-state.txt`
- `raw/nodes.txt`
- `raw/namespaces.txt`
- `raw/argocd-applications.txt`
- `raw/all-pods.txt`
- `raw/non-running-pods.txt`
- `raw/app-runtime.txt`
- `raw/monitoring-runtime.txt`
- `raw/grafana-app-dashboard-configmap.yaml`
- `raw/app-alert-rules.yaml`
- `raw/kyverno-policyreports.txt`
- `raw/ingress-nginx-runtime.txt`
- `raw/metallb-runtime.txt`
- `raw/cert-manager-runtime.txt`
- `raw/longhorn-runtime.txt`
- `raw/storageclasses.txt`
- `raw/kyverno-clusterpolicies.txt`
- `raw/prometheus-ready.txt`
- `raw/query-app-version.json`
- `raw/query-app-scrape-up.json`
- `raw/query-app-ready-replicas.json`
- `raw/query-app-firing-alerts.json`
- `raw/prometheus-app-alert-rules.json`
