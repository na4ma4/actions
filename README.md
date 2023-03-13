# GitHub Actions

Reusable workflows and other GitHub Actions things.

## Workflows

### Unit Test

```yaml
jobs:
  unittest:
    uses: na4ma4/actions/.github/workflows/unit-test.yml@v1
```

### GoReleaser

```yaml
jobs:
  goreleaser:
    if: startsWith(github.ref, 'refs/tags/v')
    needs:
      - unittest
    uses: na4ma4/actions/.github/workflows/goreleaser.yml@v1
    secrets:
      token: ${{ secrets.GO_RELEASER_GITHUB_TOKEN }}
    with:
      docker: true
```

### GitHub Release (makefiles)

```yaml
jobs:
  release:
    if: startsWith(github.ref, 'refs/tags/v')
    needs:
      - unittest
    uses: na4ma4/actions/.github/workflows/makefiles-release.yml@dev
    with:
      platforms: 'linux/amd64,windows/amd64'
      language: bare
```

Where `platforms` is a comma-separated list of platforms, defaulting to empty, and `language` specifies the supported languages (currently `bare` and `golang`, defaulting to `golang`).

## Examples

### Unit Test Workflow

```yaml
name: "CI"

on:
  pull_request:
  push:
    branches:
    - '*'
    tags:
    - 'v*'

jobs:
  unittest:
    uses: na4ma4/actions/.github/workflows/unit-test.yml@v1
```

### Unit Test with GoReleaser Workflow

```yaml
name: "CI"

on:
  pull_request:
  push:
    branches:
    - '*'
    tags:
    - 'v*'

jobs:
  unittest:
    uses: na4ma4/actions/.github/workflows/unit-test.yml@v1

  goreleaser:
    if: startsWith(github.ref, 'refs/tags/v')
    needs:
      - unittest
    uses: na4ma4/actions/.github/workflows/goreleaser.yml@v1
    secrets:
      token: ${{ secrets.GO_RELEASER_GITHUB_TOKEN }}
```
