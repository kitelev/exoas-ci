# exoas-ci

Shared reusable GitHub Actions CI for Exocortex AssetSpace repositories.

Part of the **Exocortex Knowledge Architecture** program — Phase A (RFC `78c2b7d0` §C2 + `de77fe27` VL#17).

## What it does

`.github/workflows/assetspace-ci.yml` is a `workflow_call` reusable workflow that, for the calling repo's checked-out content:

1. `validate schema --shapes-mode` — SHACL-lite shape conformance.
2. `audit ontology-imports` — cross-ontology closure resolution + SCC / DAG check.

Both run via `npx @kitelev/exocortex-cli`. **Warn-only by default** (`warn_only: true`) per the RR-B staging model (warn-only → per-repo-blocking once audit-green).

## Usage (caller repo)

```yaml
# .github/workflows/ci.yml in an AssetSpace repo
name: CI
on: [push, pull_request]
jobs:
  ci:
    uses: kitelev/exoas-ci/.github/workflows/assetspace-ci.yml@main
    with:
      warn_only: true
```

Flip `warn_only: false` to make CI blocking once the repo is audit-green.
