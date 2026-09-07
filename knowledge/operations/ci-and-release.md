---
type: Configuration
title: CI and Release
description: GitHub Actions workflows that build every service image, run checks, and cut releases.
resource: https://github.com/agentic-ai-demos/microservices-demo/blob/main/.github/workflows
tags: [ci, github-actions, release, renovate]
timestamp: 2026-09-03T15:40:08-04:00
source_files:
  - .github/workflows
  - skaffold.yaml
generated_by: catalogify/0.8.0
---
# Responsibilities

GitHub Actions workflows that build every service image, run checks, and cut releases.

# Interfaces

| Workflow group | Purpose |
| --- | --- |
| Build and push | One image per service, all five languages. |
| Checks | Linting and manifest validation on pull requests. |
| Release | Tag, build and publish a versioned set. |

# Dependencies

Builds the images that [Kubernetes Manifests](kubernetes-manifests.md) and the
[Helm Chart](helm-chart.md) reference by tag.

# Gotchas

**Most of this repository's commit history is automated dependency updates**, and they are
the reason the co-change signal in the [architecture overview](../architecture/overview.md)
groups services by language rather than by call path. When reading history for a service, expect
to filter `chore(deps)` and `fix(deps)` out before anything meaningful appears.

Release plumbing has needed its own fixes, including the load generator being wrong in
`skaffold.yaml` (`79be00ae`).

# Citations

1. `79be00ae` — Fix loadgenerator in skaffold.yaml (#2986).
