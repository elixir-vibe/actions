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

Override commands when a repository needs custom build flags:

```yaml
jobs:
  ci:
    uses: elixir-vibe/actions/.github/workflows/elixir-ci.yml@v1
    with:
      latest-command: OXC_EX_BUILD=1 mix test
      min-command: OXC_EX_BUILD=1 mix compile --warnings-as-errors && OXC_EX_BUILD=1 mix test
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
