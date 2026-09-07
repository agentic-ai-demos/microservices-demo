---
type: Configuration
title: Helm Chart
description: A parameterised chart covering the same topology, with security and mesh options.
resource: https://github.com/agentic-ai-demos/microservices-demo/blob/main/helm-chart
tags: [helm, chart, deployment]
timestamp: 2026-09-03T15:39:13-04:00
source_files:
  - helm-chart
generated_by: catalogify/0.8.0
---
# Responsibilities

A parameterised chart covering the same topology, with security and mesh options.

# Interfaces

| Value group | Purpose |
| --- | --- |
| `images` | Registry, repository and tag per service. |
| `seccompProfile`, `securityContext` | Pod hardening switches. |
| `serviceAccounts` | Workload identity wiring. |

# Dependencies

An alternative to [Kubernetes Manifests](kubernetes-manifests.md) and
[Kustomize Variants](kustomize-variants.md), covering the same twelve services.

# Gotchas

**Templating errors here fail silently until deploy.** Two separate fixes were needed for
nesting mistakes: `896f214d` for an invalid `spec.template.spec.containers` block, and
`342916ba` for `seccompProfile.enable` and `securityContext.enabled` being nested wrongly. A
mis-nested value does not error, it simply does not apply, which means a security setting can
read as enabled in `values.yaml` and be absent in the cluster.

# Citations

1. `896f214d` — fix invalid helm templating for spec.template.spec.containers block (#3031).
2. `342916ba` — nest .Values.seccompProfile.enable and .Values.securityContext.enabled correctly (#2874).
