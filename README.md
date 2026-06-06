# Elixir Vibe Actions

Shared GitHub Actions and reusable workflows for Elixir Vibe and Elixir Volt projects.

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
