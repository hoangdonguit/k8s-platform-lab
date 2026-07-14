# Runbook - Longhorn PVC Debug

## Purpose

Troubleshoot Longhorn storage and PVC provisioning.

## Check Longhorn components

`kubectl -n longhorn-system get pods,svc,deploy,ds -o wide`

Expected:

- Longhorn manager pods are Running.
- CSI plugin is Running on every node.
- CSI sidecars are Running.

## Check StorageClasses

`kubectl get storageclass -o wide`

Expected:

- `local-path` remains default.
- `longhorn` is available.
- `longhorn-static` is available.

## Check PVC

`kubectl -n <namespace> get pvc -o wide`

Describe PVC:

`kubectl -n <namespace> describe pvc <pvc-name>`

## Check PV

`kubectl get pv`

Describe PV:

`kubectl describe pv <pv-name>`

## Check Longhorn volume

`kubectl -n longhorn-system get volumes.longhorn.io -o wide`

Inspect volume:

`kubectl -n longhorn-system get volumes.longhorn.io <volume-name> -o yaml`

## Common symptoms

### PVC Pending

Possible causes:

- StorageClass name is wrong.
- Longhorn CSI is not ready.
- Not enough available disk.
- Node/disk scheduling disabled.

### Pod stuck ContainerCreating

Possible causes:

- Volume attach failed.
- CSI node plugin issue.
- Longhorn volume not healthy.

### Volume degraded

Possible causes:

- One replica unavailable.
- Node unavailable.
- Disk pressure or scheduling issue.

## Lab rule

Longhorn is not the default StorageClass.

Stateful workloads must explicitly set:

`storageClassName: longhorn`
