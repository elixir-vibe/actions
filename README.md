# Elixir Vibe Actions

Shared GitHub Actions and reusable workflows for Elixir Vibe and Elixir Volt projects.

How our repositories present themselves — README structure, badges, descriptions, cross-linking — is codified in [README-STYLE.md](README-STYLE.md).

## Reusable Elixir CI

```yaml
name: CI

on:
  push:
    branches: [main, master]
  pull_request:
    branches: [main, master]

permissions:
  contents: read

jobs:
  ci:
    uses: elixir-vibe/actions/.github/workflows/elixir-ci.yml@v1
```

The default CI runs:

- Elixir `1.20` / OTP `29` with `mix ci`
- Elixir `1.19` / OTP `27` with `mix compile --warnings-as-errors && mix test`

Override commands when a repository needs custom checks:

```yaml
jobs:
  ci:
    uses: elixir-vibe/actions/.github/workflows/elixir-ci.yml@v1
    with:
      latest-command: mix ci
      min-command: mix compile --warnings-as-errors && mix test
```

## Reusable Rustler CI

Use this for Elixir projects that build Rustler NIFs or run Cargo checks:

```yaml
name: CI

on:
  push:
    branches: [main, master]
  pull_request:
    branches: [main, master]

permissions:
  contents: read

jobs:
  ci:
    uses: elixir-vibe/actions/.github/workflows/elixir-rustler-ci.yml@v1
    with:
      rust-toolchain: stable
      rust-cache-workspaces: native/my_app_nif -> target
      extra-env: |
        MY_APP_BUILD=1
```

For projects with submodules or Ubuntu system package dependencies, pass them through the workflow inputs:

```yaml
jobs:
  ci:
    uses: elixir-vibe/actions/.github/workflows/elixir-rustler-ci.yml@v1
    with:
      checkout-submodules: recursive
      apt-packages: libfontconfig1-dev libfreetype6-dev
      rust-cache-workspaces: native/my_app_nif -> target
```

For projects with multiple Rust crates, list each workspace:

```yaml
jobs:
  ci:
    uses: elixir-vibe/actions/.github/workflows/elixir-rustler-ci.yml@v1
    with:
      rust-toolchain: "1.95.0"
      rust-profile: default
      rust-cache-workspaces: |
        . -> target
        native/my_app_lint_nif -> target
        native/my_app_fmt_nif -> target
      extra-env: |
        MY_APP_BUILD=1
```

## Reusable Rustler precompiled release

Use this from tag-triggered release workflows that build `rustler_precompiled` archives:

```yaml
name: Build precompiled NIFs

on:
  push:
    tags: ["v*"]

jobs:
  build_release:
    uses: elixir-vibe/actions/.github/workflows/elixir-rustler-release.yml@v1
    with:
      project-name: my_app_nif
```

Override `jobs`, `nif-versions`, `project-dir`, or `cargo-args` for repositories with custom target matrices. Native system dependencies and build-time environment variables are also centralized:

```yaml
jobs:
  build_release:
    uses: elixir-vibe/actions/.github/workflows/elixir-rustler-release.yml@v1
    permissions:
      contents: write
      id-token: write
      attestations: write
      artifact-metadata: write
    with:
      project-name: my_app_nif
      apt-packages: libxkbcommon-dev libxkbcommon-x11-dev
      extra-env: |
        MY_APP_BUILD=1
      attest-artifacts: true
```

For a tagged `rustler_precompiled` release, the shared workflow can generate and attach the mandatory checksum after every matrix artifact has been published:

```yaml
    with:
      project-name: my_app_nif
      checksum-module: MyApp.Native
      checksum-file: checksum-Elixir.MyApp.Native.exs
      checksum-prepare-command: MY_APP_BUILD=1 mix compile
```

Checksum generation only runs for tags and remains disabled unless both checksum inputs are provided.

## Setup Elixir composite action

Use this when a repository needs custom jobs but wants the shared setup/cache steps:

```yaml
steps:
  - uses: actions/checkout@v4
  - uses: elixir-vibe/actions/setup-elixir@v1
    with:
      elixir-version: "1.20"
      otp-version: "29"
  - run: mix ci
```
