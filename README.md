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
  goreleaser:
    uses: na4ma4/actions/.github/workflows/goreleaser.yml@v1
    secrets:
      token: ${{ secrets.GO_RELEASER_GITHUB_TOKEN }}
    with:
      docker: true
```

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
