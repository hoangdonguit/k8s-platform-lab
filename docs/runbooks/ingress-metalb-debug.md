# Runbook - Ingress and MetalLB Debug

## Purpose

Troubleshoot HTTP access through ingress-nginx and MetalLB.

## Current baseline

- ingress-nginx LoadBalancer IP: `192.168.0.241`
- Demo app host: `demo.p2.local`
- Demo app namespace: `apps`

## Check ingress-nginx

`kubectl -n ingress-nginx get svc,pod,ds -o wide`

Expected:

- ingress-nginx controller pods are Running.
- Service type is LoadBalancer.
- External IP is assigned by MetalLB.

## Check MetalLB

`kubectl -n metallb-system get pods -o wide`

`kubectl -n metallb-system get ipaddresspool,l2advertisement -o wide`

Expected:

- Controller is Running.
- Speakers are Running on nodes.
- IPAddressPool contains the private LoadBalancer range.

## Check application ingress

`kubectl -n apps get ingress -o wide`

Expected:

- Ingress class is `nginx`.
- Host is configured.
- Address is `192.168.0.241`.

## Check service endpoints

`kubectl -n apps get svc,endpoints,endpointslice -o wide`

Expected:

- Service selector matches Pod labels.
- EndpointSlice points to the application Pod IP and target port.

## Test from cluster nodes

The MetalLB IP is on the cluster private network. The admin machine may not route to it directly.

Valid test from a cluster node:

`curl -si -H 'Host: demo.p2.local' http://192.168.0.241/`

Health check:

`curl -si -H 'Host: demo.p2.local' http://192.168.0.241/healthz`

## Test from inside the cluster

`kubectl -n apps run curl-check --rm -i --restart=Never --image=curlimages/curl:8.10.1 -- curl -si http://gitops-cicd-demo-app/healthz`

## Common symptoms

### curl timeout from admin machine

Likely cause:

- Admin machine cannot route to cluster private network.

This is not necessarily an application failure.

### HTTP 404

Likely cause:

- Host header does not match Ingress rule.
- Wrong Ingress class.
- Ingress not reconciled yet.

### HTTP 503

Likely cause:

- Service has no ready endpoints.
- Pod readiness probe failing.
- Service selector does not match Pod labels.
