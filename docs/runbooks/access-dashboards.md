# Runbook - Access Dashboards

## Purpose

Access internal platform dashboards safely through local port-forwarding.

Dashboards are not exposed publicly by default.

## ArgoCD

Port-forward:

`kubectl -n argocd port-forward svc/argocd-server 8080:443 --address 127.0.0.1`

Open:

`https://127.0.0.1:8080`

## Grafana

Port-forward:

`kubectl -n monitoring port-forward svc/kube-prometheus-stack-grafana 3000:80 --address 127.0.0.1`

Open:

`http://127.0.0.1:3000`

Default lab account:

- Username: `admin`
- Password: configured in kube-prometheus-stack values

## Longhorn

Port-forward:

`kubectl -n longhorn-system port-forward svc/longhorn-frontend 8081:80 --address 127.0.0.1`

Open:

`http://127.0.0.1:8081`

## Security note

For this lab, dashboard access uses port-forwarding instead of public Ingress exposure.

Reason:

- Avoid exposing admin dashboards.
- Avoid relying on weak default credentials.
- Keep the lab safe while still allowing operational visibility.

Future hardening may add Ingress, TLS, authentication, and restricted network access.
