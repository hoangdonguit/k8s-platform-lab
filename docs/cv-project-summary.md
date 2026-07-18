# CV Project Summary - k8s-platform-lab

## Project name

`k8s-platform-lab`

## One-line summary

Built a 3-node K3s platform using GitOps, ingress, monitoring, storage, policy-as-code, application onboarding, custom metrics, alerting, fire-drill validation, and Grafana dashboard-as-code.

## Role

Platform Engineering / DevOps project.

This project focuses on building and operating a Kubernetes platform, not only deploying a single application.

## Architecture

The platform includes:

- K3s 3-node cluster
- Cilium CNI
- ArgoCD App-of-Apps
- ingress-nginx
- MetalLB LoadBalancer
- cert-manager
- kube-prometheus-stack
- Grafana
- Alertmanager
- Longhorn CSI storage
- Kyverno policy-as-code

The platform onboards a separate GitOps CI/CD application from another repository:

- App repository: `gitops-cicd-k3s-lab`
- Platform repository: `k8s-platform-lab`
- App namespace: `apps`
- App image: `hoangdonguit/gitops-cicd-demo-app:sha-b10ebf1`

## What I built

### Platform bootstrap

Built a Kubernetes platform with:

- 3 Ready K3s nodes
- GitOps-based platform deployment
- ArgoCD root app and child apps
- platform namespaces
- ingress and private LoadBalancer support
- monitoring stack
- distributed storage baseline
- policy baseline

### Application onboarding

Onboarded an external Flask app from a separate CI/CD repository.

The app is deployed through ArgoCD using:

- Kustomize overlay
- separate Git repo source
- automated sync
- ingress exposure
- ServiceMonitor
- PrometheusRule
- Grafana dashboard ConfigMap

### Observability

Implemented two observability layers:

Kubernetes-level metrics:

- deployment replicas
- pod readiness
- container CPU
- container memory
- resource requests

Application-level metrics:

- app version
- HTTP request counter
- request duration histogram
- p95 latency by route
- 5xx error ratio

### Alerting

Implemented Prometheus alert rules for:

- app scrape target down
- no ready pods
- high 5xx error ratio
- high p95 latency

Also performed a controlled alert fire-drill:

- temporary GitOps-managed rule
- alert moved to `firing`
- rule removed through GitOps
- baseline alert rules remained healthy

### Dashboard

Created a Grafana dashboard-as-code for the onboarded app.

Dashboard panels include:

- app version
- scrape target status
- ready replicas
- firing alerts
- request rate by route
- p95 latency by route
- 5xx ratio
- CPU usage
- memory working set

### Policy-as-code

Added Kyverno baseline policies:

- require common workload labels
- disallow privileged containers
- require CPU/memory requests

Validated that the onboarded app passes policy reports with no failures.

### Storage

Installed Longhorn and validated PVC behavior with smoke test evidence.

## Evidence-driven workflow

The project records evidence for major milestones:

- K3s and Cilium bootstrap
- ArgoCD App-of-Apps
- ingress-nginx and MetalLB
- cert-manager
- kube-prometheus-stack
- Longhorn
- Kyverno
- app onboarding
- workload observability
- custom app metrics
- alerting baseline
- alert fire-drill
- Grafana dashboard
- final platform review

## CV bullet options

### Short version

Built a 3-node K3s platform with ArgoCD App-of-Apps, Cilium, ingress-nginx, MetalLB, cert-manager, kube-prometheus-stack, Longhorn, and Kyverno; onboarded a separate GitOps CI/CD Flask app with custom Prometheus metrics, alert rules, fire-drill validation, and Grafana dashboard-as-code.

### Strong version

Designed and operated a 3-node Kubernetes platform on K3s using GitOps with ArgoCD App-of-Apps; implemented ingress-nginx + MetalLB, cert-manager, kube-prometheus-stack, Longhorn, and Kyverno policy-as-code. Onboarded an external CI/CD Flask app, exposed it via ingress, added custom Prometheus metrics, ServiceMonitor scraping, PrometheusRule alerting, controlled alert fire-drill validation, and Grafana dashboard-as-code with evidence-driven documentation.

### Interview version

This project demonstrates a platform engineering workflow: build the Kubernetes platform, deploy platform services through GitOps, onboard a real app from a separate CI/CD repository, validate policy compliance, expose metrics, create alert rules, test alert firing safely, and provide a Grafana dashboard for operations.

## Skills demonstrated

- Kubernetes operations
- GitOps with ArgoCD
- Kustomize
- Helm-rendered manifests
- ingress-nginx
- MetalLB
- Prometheus Operator
- ServiceMonitor
- PrometheusRule
- Grafana dashboard-as-code
- Alertmanager baseline
- Longhorn storage
- Kyverno policy-as-code
- platform evidence and runbook writing
- troubleshooting and debugging

## Final status

CV-ready.

The platform is healthy and the repository contains operational evidence, runbooks, summaries, dashboard configuration, alert rules, and final review proof.
