# meta

This repo contains my meta configuration files:

- goreleaser configurations
- github actions workflows

## Related docs

- [GitHub: Reusing Workflows](https://docs.github.com/en/actions/learn-github-actions/reusing-workflows)
- [GoReleaser: includes](https://goreleaser.com/customization/includes/)

## Usage

### GoReleaser release

```yaml
# .goreleaser.yml
includes:
  - from_url:
      url: asphaltbuffet/meta/main/goreleaser.yaml

variables:
  main: ""
  binary_name: ""
  description: ""
  github_url: ""
  maintainer: "Ben Lechlitner"
  homepage: "https://charm.sh/"
  brew_commit_author_name: ""
  brew_commit_author_email: ""
  brew_owner: asphaltbuffet
  docker_io_registry_owner: asphaltbuffet
  ghcr_io_registry_owner: asphaltbuffet
```

> You can override the variables you need

```yaml
# .github/workflows/goreleaser.yml
name: goreleaser

on:
  push:
    tags:
      - v*.*.*

concurrency:
  group: goreleaser
  cancel-in-progress: true

jobs:
  goreleaser:
    uses: asphaltbuffet/meta/.github/workflows/goreleaser.yml@main
    secrets:
      docker_username: ${{ secrets.DOCKERHUB_USERNAME }}
      docker_token: ${{ secrets.DOCKERHUB_TOKEN }}
      gh_pat: ${{ secrets.PERSONAL_ACCESS_TOKEN }}
      goreleaser_key: ${{ secrets.GORELEASER_KEY }
```

You'll need to set the secrets used.

### GoReleaser snapshot

```yaml
# .github/workflows/snapshot.yml
name: snapshot

on: [push, pull_request]

jobs:
  snapshot:
    uses: asphaltbuffet/meta/.github/workflows/snapshot.yml@main
    secrets:
      goreleaser_key: ${{ secrets.GORELEASER_KEY }}
```

### GoReleaser nightly

```yaml
# .github/workflows/nightly.yml
name: nightly

on: push

jobs:
  nightly:
    uses: asphaltbuffet/meta/.github/workflows/nightly.yml@main
    secrets:
      docker_username: ${{ secrets.DOCKERHUB_USERNAME }}
      docker_token: ${{ secrets.DOCKER_PASSWORD }}
      goreleaser_key: ${{ secrets.GORELEASER_KEY }}
```

```yaml
# .github/workflows/pr-comment.yml
name: pr-comment

on:
  workflow_run:
    workflows: [build]
    types: [completed]

jobs:
  pr-comment:
    uses: asphaltbuffet/meta/.github/workflows/pr-comment.yml@main
```
