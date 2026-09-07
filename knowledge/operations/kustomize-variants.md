---
type: Configuration
title: Kustomize Variants
description: Kustomize overlays and components compose optional deployment variants around the baseline manifests.
resource: https://github.com/agentic-ai-demos/microservices-demo/blob/main/kustomize/kustomization.yaml
tags: [kustomize, kubernetes, variants]
timestamp: 2026-09-03T17:10:06-04:00
source_files:
  - kustomize/kustomization.yaml
  - kustomize/base/kustomization.yaml
  - kustomize/README.md
  - terraform/main.tf
  - kustomize/components/google-cloud-operations/kustomization.yaml
  - kustomize/components/shopping-assistant/kustomization.yaml
  - kustomize/components/memorystore/kustomization.yaml
  - kustomize/components/spanner/kustomization.yaml
  - kustomize/components/network-policies/kustomization.yaml
generated_by: catalogify/0.7.0
open_questions:
  - "Which Kustomize components are release-blocking variants versus examples maintained on a best-effort basis?"
---

# Responsibilities

Kustomize provides composable deployment variants for the baseline application. Components add or alter observability, service mesh, cart database backends, image registry/tag rules, network policies, non-public frontend exposure, Cymbal branding, shared sessions, and the shopping assistant.

# Interfaces

| Symbol | Purpose |
| --- | --- |
| `kustomization.yaml` | Top-level composition entry point. |
| `base/kustomization.yaml` | Baseline resources used by overlays. |
| `components/google-cloud-operations/kustomization.yaml` | Observability component. |
| `components/shopping-assistant/kustomization.yaml` | Optional AI assistant service wiring. |
| `components/memorystore/kustomization.yaml` | External Redis/Memorystore cart backend variant. |
| `components/spanner/kustomization.yaml` | Spanner cart backend variant. |
| `components/network-policies/kustomization.yaml` | NetworkPolicy variant. |

# Dependencies

Kustomize depends on [Kubernetes Manifests](kubernetes-manifests.md) and has historically co-changed with [Helm Chart](helm-chart.md), especially where both deployment systems expose the same observability and security features.

# Gotchas

* Cloud Trace service naming had to be fixed across Helm templates and Kustomize operations components, so observability labels/env must remain consistent across deployment systems (`02015ee8`).
* A rollback touched Kustomize cloud-ops sandbox files and Terraform together; cross-tool deployment variants can share failure modes even without direct imports (`30c62eb2`).

# Citations

1. `02015ee8` - fix: display service name in Cloud Trace (#2350).
2. `30c62eb2` - fix: rollback customizations made in #1566 (#1573).
