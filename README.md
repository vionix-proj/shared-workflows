# shared-workflows

Reusable GitHub Actions workflows for organization-wide automation.

## Reusable workflow: `docker-multiarch-cicd`

This workflow preserves the original multi-job Docker pipeline (PR build, PR cleanup, publish-on-merge, and tag release) while exposing top-level environment values as `workflow_call` inputs.

```yaml
name: docker-image-ci

on:
  pull_request:
    types: [opened, synchronize, reopened, closed]
  push:
    tags:
      - 'v*'

jobs:
  docker-multiarch:
    uses: vionix-proj/shared-workflows/.github/workflows/docker-multiarch-cicd.yaml@main
    with:
      registry: ghcr.io
      target-service: gha-fix
      platforms: linux/amd64,linux/arm64
      docker-compose-file-name: docker-compose.yaml
      dockerhub-registry: docker.io
      dockerhub-namespace: ${{ github.repository_owner }}
      push-attestations: "false"
    secrets: inherit
```

### Inputs

- `registry` (default: `ghcr.io`)
- `target-service` (default: `gha-fix`)
- `platforms` (default: `linux/amd64,linux/arm64`)
- `docker-compose-file-name` (default: `docker-compose.yaml`)
- `dockerhub-registry` (default: `docker.io`)
- `dockerhub-namespace` (default: `${{ github.repository_owner }}`)
- `push-attestations` (default: `"false"`)
