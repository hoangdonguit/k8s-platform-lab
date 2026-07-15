# Phase 2 - Workload Observability Baseline Evidence

## Goal

Verify that the platform monitoring stack can observe the onboarded `gitops-cicd-demo-app` workload.

This checkpoint uses Kubernetes-level metrics from kube-state-metrics and kubelet/cAdvisor. The application does not expose custom Prometheus metrics yet.

## Result

PASS if Prometheus returns data for the onboarded app.

Validated signals:

- Deployment available replicas
- Pod information
- Pod readiness
- CPU and memory resource requests
- Container memory usage
- Container CPU usage rate

## Application

- Name: `gitops-cicd-demo-app`
- Namespace: `apps`
- Image: `hoangdonguit/gitops-cicd-demo-app:sha-c73671f`
- Ingress host: `demo.p2.local`
- Ingress LoadBalancer IP: `192.168.0.241`

## Monitoring stack

The platform uses kube-prometheus-stack, including:

- Prometheus
- Alertmanager
- Grafana
- kube-state-metrics
- node-exporter
- Prometheus Operator

## Why this matters

This proves the platform can observe a real onboarded application using standard Kubernetes metrics.

The current checkpoint validates platform-level observability. A later enhancement can add custom `/metrics` instrumentation to the Flask app and register it with a ServiceMonitor.

## Evidence files

- `raw/argocd-applications.txt`
- `raw/app-runtime.txt`
- `raw/monitoring-runtime.txt`
- `raw/monitoring-crds.txt`
- `raw/non-running-pods.txt`
- `raw/prometheus-ready.txt`
- `raw/query-app-deployment-available.json`
- `raw/query-app-pod-info.json`
- `raw/query-app-pod-ready.json`
- `raw/query-app-resource-requests.json`
- `raw/query-app-container-memory.json`
- `raw/query-app-container-cpu.json`
