# Runbook - Kyverno Policy Debug

## Purpose

Troubleshoot Kyverno policy-as-code behavior.

## Check Kyverno

`kubectl -n kyverno get pods,svc,deploy -o wide`

Expected:

- Admission controller is Running.
- Background controller is Running.
- Cleanup controller is Running.
- Reports controller is Running.

## Check policies

`kubectl get clusterpolicy -o wide`

Expected:

- Policies are Ready.
- Admission and background are enabled.

## Check reports

`kubectl get policyreport -A`

Detailed reports:

`kubectl get policyreport -n apps -o yaml`

## Current baseline policies

- `p2-baseline-require-common-labels`
- `p2-baseline-disallow-privileged-apps`
- `p2-baseline-require-resource-requests-apps`

## Current mode

Policies start in `Audit` mode.

Meaning:

- Non-compliant resources are allowed.
- Violations are written to PolicyReport.
- The platform can observe policy impact before enforcing.

## Common symptoms

### Resource is allowed but report shows fail

This is expected in Audit mode.

### Resource is blocked

Possible causes:

- Policy was moved to Enforce.
- Another admission controller blocked it.
- Namespace is covered by stricter policy.

### ArgoCD OutOfSync on Kyverno

Possible causes:

- Kyverno defaulted policy fields.
- CRD defaulting changed live resources.
- Helm hook or test resources were rendered into Git.

Lab fix used:

- Normalize Kyverno default fields in Git.
- Render Kyverno with `--no-hooks --skip-tests`.
- Ignore safe CRD/status drift in the ArgoCD Application.
