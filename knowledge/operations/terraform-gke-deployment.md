---
type: Configuration
title: Terraform GKE Deployment
description: Terraform provisions Google Cloud APIs, an Autopilot GKE cluster, optional Memorystore, and applies the manifests.
resource: https://github.com/agentic-ai-demos/microservices-demo/blob/main/terraform/main.tf
tags: [terraform, gke, infrastructure]
timestamp: 2026-09-03T15:39:28-04:00
source_files:
  - terraform/main.tf
  - terraform/memorystore.tf
  - terraform/output.tf
  - terraform/providers.tf
  - terraform/variables.tf
generated_by: catalogify/0.7.0
open_questions:
  - "Is the Terraform path intended to be idempotent for repeated demo creation/destruction in shared projects, or mainly a first-run quickstart?"
---

# Responsibilities

Terraform enables required Google Cloud APIs, creates an Autopilot GKE cluster, fetches credentials through the gcloud module, applies Kubernetes manifests, waits for pods, and optionally provisions Memorystore for the cart backend. It is an infrastructure wrapper around the manifest deployment path rather than a separate application definition.

# Interfaces

| Symbol | Purpose |
| --- | --- |
| `module.enable_google_apis` | Enables container, monitoring, trace, profiler, and optional Redis APIs. |
| `google_container_cluster.my_cluster` | Creates the Autopilot cluster. |
| `module.gcloud` | Fetches cluster credentials. |
| `null_resource.apply_deployment` | Applies the configured manifest path with `kubectl apply -k`. |
| `null_resource.wait_conditions` | Waits for metrics API and pods to become ready. |
| `google_redis_instance` | Optional Memorystore Redis backend. |

# Dependencies

Terraform applies [Kustomize Variants](kustomize-variants.md) or [Kubernetes Manifests](kubernetes-manifests.md), depending on `filepath_manifest`. Cart backend settings connect it to [Cart Service](../services/cartservice.md).

# Gotchas

* Terraform provider major-version changes have been reverted, so provider upgrades should be validated against both root Terraform and `.github/terraform` validation code (`614b8f83`).
* Deployment customization rollback touched Terraform and Kustomize together; do not treat IaC and manifest overlay changes as independent when they configure the same variant (`30c62eb2`).

# Citations

1. `614b8f83` - Revert "chore(deps): update terraform google to v6 (#2711)" (#2714).
2. `30c62eb2` - fix: rollback customizations made in #1566 (#1573).
