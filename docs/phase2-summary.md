# Phase 2 Summary - Application Onboarding and Observability

## Purpose

Phase 2 extends the Kubernetes platform from a tool-focused MVP into an application-ready platform.

The main goal was to prove that `k8s-platform-lab` can onboard, expose, secure, and observe a real application from a separate CI/CD repository.

## Related repositories

### Project 1 - gitops-cicd-k3s-lab

Role:

- Application source code
- Flask demo app
- Dockerfile
- GitHub Actions CI/CD
- Docker Hub image publishing
- Kustomize app manifests
- Platform-specific overlay for Project 2

Important path:

- `k8s/overlays/p2-platform`

### Project 2 - k8s-platform-lab

Role:

- Kubernetes platform
- ArgoCD App-of-Apps
- ingress-nginx
- MetalLB
- cert-manager
- kube-prometheus-stack
- Longhorn
- Kyverno
- Application onboarding and platform evidence

Important ArgoCD app:

- `p2-portfolio-gitops-cicd-demo`

## Final application state

Application:

- Name: `gitops-cicd-demo-app`
- Namespace: `apps`
- Image: `hoangdonguit/gitops-cicd-demo-app:sha-b10ebf1`
- Ingress host: `demo.p2.local`
- Ingress LoadBalancer IP: `192.168.0.241`
- Metrics path: `/metrics`

Runtime:

- Deployment available: `1/1`
- Pod status: `Running`
- ServiceMonitor: present
- ArgoCD status: `Synced` / `Healthy`

## GitOps flow

The application is deployed through ArgoCD from Project 1 into Project 2.

Flow:

1. Code changes are committed to `gitops-cicd-k3s-lab`.
2. GitHub Actions builds and pushes a Docker image.
3. GitHub Actions updates the Kustomize image tag in Git.
4. ArgoCD in `k8s-platform-lab` detects the new Git revision.
5. ArgoCD syncs the updated application into the `apps` namespace.
6. ingress-nginx exposes the app through MetalLB.
7. Prometheus scrapes the app through a ServiceMonitor.

## Traffic flow

External/private-network traffic path:

1. Client sends request with host `demo.p2.local`.
2. MetalLB exposes ingress-nginx at `192.168.0.241`.
3. ingress-nginx routes the request.
4. Kubernetes Service forwards traffic to the app Pod.
5. Flask app responds from port `8080`.

Internal service path:

1. Cluster workload calls `http://gitops-cicd-demo-app`.
2. Kubernetes Service forwards to the ready endpoint.
3. Pod handles request on port `8080`.

## Observability result

Phase 2 validated two observability levels.

### Kubernetes-level observability

Prometheus can observe the workload through:

- kube-state-metrics
- kubelet / cAdvisor

Validated signals:

- Deployment available replicas
- Pod info
- Pod readiness
- CPU and memory requests
- Container memory working set
- Container CPU usage rate

Evidence:

- `evidence/phase2-workload-observability-baseline`

### Application-level observability

The Flask application now exposes custom Prometheus metrics.

Custom metrics:

- `gitops_cicd_demo_app_info`
- `gitops_cicd_demo_http_requests_total`
- `gitops_cicd_demo_http_request_duration_seconds`

Prometheus integration:

- ServiceMonitor: `apps/gitops-cicd-demo-app`
- Target: `/metrics`
- Target health: `up`
- Scrape error: empty

The metric label was polished from `endpoint` to `route` to avoid collision with Prometheus target labels.

Evidence:

- `evidence/phase2-custom-app-metrics`
- `evidence/phase2-custom-app-metrics-route-label`

## Policy validation

Kyverno baseline policies remained compatible with the onboarded application.

Relevant policy expectations:

- Workload labels are present.
- Privileged containers are not used.
- CPU and memory requests are defined.

The application passed the current Kyverno baseline.

Evidence:

- `evidence/phase2-portfolio-app-onboarding`
- `evidence/phase1-kyverno-policy-baseline`

## Security and runtime hardening

The onboarded application keeps the security baseline from Project 1:

- Runs as non-root user `10001`
- Disables privilege escalation
- Drops Linux capabilities
- Uses `RuntimeDefault` seccomp profile
- Disables service account token automount
- Uses read-only root filesystem
- Mounts `/tmp` as memory-backed `emptyDir`
- Defines CPU and memory requests/limits

## Important troubleshooting notes

### Local access to MetalLB IP

The admin machine may not be able to directly route to `192.168.0.241`.

This is not an application failure.

Valid tests:

- curl from cluster nodes
- curl from inside the cluster
- kubectl port-forward for local-only access

### Prometheus label collision

The first custom metric implementation used `endpoint` as an app metric label.

Prometheus also adds a target label named `endpoint`, so Prometheus renamed the application label to `exported_endpoint`.

Fix:

- Changed app metric label from `endpoint` to `route`.

Result:

- Cleaner Prometheus queries
- Easier dashboard and alert design

## Evidence index for Phase 2

- `evidence/phase2-portfolio-app-onboarding`
- `evidence/phase2-workload-observability-baseline`
- `evidence/phase2-custom-app-metrics`
- `evidence/phase2-custom-app-metrics-route-label`

## CV-ready description

Built and extended a 3-node K3s platform with ArgoCD App-of-Apps, ingress-nginx, MetalLB, cert-manager, kube-prometheus-stack, Longhorn, and Kyverno; onboarded a separate GitOps CI/CD Flask application through ArgoCD, exposed it via ingress-nginx/MetalLB, validated Kyverno policy compliance, and implemented end-to-end Prometheus observability with custom application metrics and ServiceMonitor scraping.

## Final status

Phase 2 is complete.

The platform can now:

- deploy a real external application through GitOps
- expose the workload through ingress
- validate policy compliance
- observe workload-level metrics
- scrape application-level custom metrics
- preserve operational evidence in Git
