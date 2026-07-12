# Phase 1 - Longhorn PVC Smoke Evidence

## Goal

Deploy Longhorn as the distributed block storage layer for the K3s platform lab and verify dynamic PVC provisioning.

## Result

PASS.

Longhorn was deployed through ArgoCD and reached `Synced` / `Healthy`.

A smoke PVC was created with `storageClassName: longhorn`. A writer pod wrote data into the mounted volume. After deleting the writer pod, a reader pod mounted the same PVC and successfully read the original data.

## Components validated

- Longhorn manager
- Longhorn UI
- Longhorn CSI plugin
- CSI attacher
- CSI provisioner
- CSI resizer
- CSI snapshotter
- Engine image daemonset
- Instance manager
- Longhorn StorageClass
- Dynamic PV/PVC provisioning
- Data persistence across pod recreation

## StorageClasses

The cluster keeps `local-path` as the default StorageClass.

Longhorn is installed as non-default StorageClasses:

- `longhorn`
- `longhorn-static`

PVC tests explicitly use `storageClassName: longhorn`.

## Smoke test flow

1. Create PVC `apps/longhorn-smoke-pvc`.
2. Create writer pod `apps/longhorn-smoke-writer`.
3. Writer pod writes data to `/data/proof.txt`.
4. Delete writer pod.
5. Create reader pod `apps/longhorn-smoke-reader`.
6. Reader pod mounts the same PVC.
7. Reader pod successfully reads `/data/proof.txt`.

## Validation result

Persisted data proof:

- `longhorn-pvc-pass 2026-07-12T05:52:06+00:00 from writer pod`

Longhorn volume state:

- Volume name: `pvc-174da5ba-4e7f-43b6-b815-4beef1c667f7`
- State: `attached`
- Robustness: `healthy`
- Size: `1Gi`
- Attached node during smoke test: `p2-node-2`

## Why this matters

This proves that the platform can dynamically provision persistent storage through Longhorn and preserve data across pod recreation. This is an important baseline before deploying stateful workloads such as databases, queues, or application services that require durable volumes.

## Evidence files

- `raw/argocd-applications.txt`
- `raw/longhorn-resources.txt`
- `raw/storageclasses.txt`
- `raw/smoke-pvc-pod.txt`
- `raw/persistent-volumes.txt`
- `raw/longhorn-smoke-pvc-describe.txt`
- `raw/longhorn-smoke-pv-describe.txt`
- `raw/longhorn-volumes.txt`
- `raw/longhorn-volume.yaml`
- `raw/persisted-data-proof.txt`
- `raw/nodes.txt`
- `raw/non-running-pods.txt`
