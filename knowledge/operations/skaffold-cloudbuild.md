---
type: Pipeline
title: Skaffold and Cloud Build
description: Skaffold builds all service images and deploys manifests locally or through Cloud Build.
resource: https://github.com/agentic-ai-demos/microservices-demo/blob/main/skaffold.yaml
tags: [skaffold, cloud-build, build, deployment]
timestamp: 2026-09-03T15:38:24-04:00
source_files:
  - skaffold.yaml
  - cloudbuild.yaml
generated_by: catalogify/0.7.0
open_questions:
  - "Are both `app` and `loadgenerator` Skaffold configs required in all release paths, or may release builds omit load generation?"
---

# Responsibilities

Skaffold defines the local build/deploy graph for service images and Kubernetes manifests. Cloud Build wraps Skaffold for GKE deployment by fetching cluster credentials and running `skaffold run` with a project-specific default repository.

# Interfaces

| Symbol | Purpose |
| --- | --- |
| `artifacts` | Lists service image build contexts. |
| `tagPolicy.gitCommit` | Tags images by git commit. |
| `manifests.kustomize.paths` | Deploys `kubernetes-manifests` by default. |
| `profile gcb` | Uses Google Cloud Build for remote builds. |
| `profile debug` | Switches cartservice to `Dockerfile.debug`. |
| `profile network-policies` | Adds the Kustomize network policies component. |
| `cloudbuild.yaml` | Deploys to a GKE cluster via Skaffold. |

# Dependencies

Skaffold builds all [Services](../services/) and deploys [Kubernetes Manifests](kubernetes-manifests.md), with optional Kustomize profile additions. Cloud Build depends on a target GKE cluster and IAM permissions outside this repository.

# Gotchas

* Platform support is cross-service: an arm64 support change was reverted across Skaffold and every Dockerfile, so architecture changes must be tested end to end (`ed7b9419`).

# Citations

1. `ed7b9419` - Revert "Add support for arm64 (#2589)".
