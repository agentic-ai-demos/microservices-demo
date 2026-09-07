---
type: Pipeline
title: GitHub Actions Pipelines
description: GitHub Actions validate code, manifests, Terraform, Helm, releases, and pull-request deployments.
resource: https://github.com/agentic-ai-demos/microservices-demo/blob/main/.github/workflows/ci-main.yaml
tags: [github-actions, ci, release]
timestamp: 2026-09-03T15:40:08-04:00
source_files:
  - .github/workflows/ci-main.yaml
  - .github/workflows/ci-pr.yaml
  - .github/workflows/deploy-pr.yaml
  - .github/workflows/helm-chart-ci.yaml
  - .github/workflows/kubevious-manifests-ci.yaml
  - .github/workflows/kustomize-build-ci.yaml
  - .github/workflows/terraform-validate-ci.yaml
  - .github/workflows/make-release.yaml
generated_by: catalogify/0.7.0
open_questions:
  - "Which GitHub Actions checks are release gates versus advisory validation for optional deployment formats?"
---

# Responsibilities

GitHub Actions provide repository CI for main and pull requests, manifest validation, Kustomize builds, Helm chart checks, Terraform validation, PR deployment, cleanup, and release automation. Recent history shows frequent dependency-action updates, so these workflows are active maintenance surfaces.

# Interfaces

| Symbol | Purpose |
| --- | --- |
| `ci-main.yaml` | Main branch continuous integration. |
| `ci-pr.yaml` | Pull-request continuous integration. |
| `deploy-pr.yaml` | Pull-request deployment workflow. |
| `helm-chart-ci.yaml` | Helm chart validation. |
| `kubevious-manifests-ci.yaml` | Kubernetes manifest validation. |
| `kustomize-build-ci.yaml` | Kustomize build validation. |
| `terraform-validate-ci.yaml` | Terraform validation. |
| `make-release.yaml` | Release automation. |

# Dependencies

The workflows exercise [Skaffold and Cloud Build](/operations/skaffold-cloudbuild.md), [Kubernetes Manifests](/operations/kubernetes-manifests.md), [Kustomize Variants](/operations/kustomize-variants.md), [Helm Chart](/operations/helm-chart.md), and [Terraform GKE Deployment](/operations/terraform-gke-deployment.md).

# Key files

| Path | Role |
| --- | --- |
| `.github/workflows/README.md` | Workflow documentation. |
| `.github/workflows/*.yaml` | CI, validation, deployment, cleanup, and release jobs. |
