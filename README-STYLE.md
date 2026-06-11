# README & Repository Presentation Guide

How repositories in the [Elixir Vibe](https://github.com/elixir-vibe),
[Elixir Volt](https://github.com/elixir-volt), and
[OpenPencil](https://github.com/open-pencil) organizations present
themselves: README structure, badges, descriptions, and cross-linking.
The goal is that any repo, found in isolation, reads as part of one
deliberate body of work — and points back to it.

## Voice

1. **Thesis sentences, not slogans.** The opening line states what the
   project is and the one idea behind it, as a plain claim. Good: *"a
   design is a node tree, not a picture"*, *"JS runtimes are
   GenServers"*. Bad: *"Supercharge your design workflow!"*. If a line
   would fit on a conference tote bag, rewrite it.
2. **Every claim is followed by its mechanism.** "Replay is
   re-evaluation" is allowed because the next sentence says how
   (templates are pure functions of assigns). A capability with no
   "how" reads as marketing.
3. **Honest status.** Experimental projects say so in the first screen
   (blockquote or `> [!WARNING]`). Known limits get their own section
   when they matter. A README that hides its doubts is marketing.
4. **Verb-first feature bullets.** "Opens `.fig` files natively", not
   "Native `.fig` file support".
5. **Numbers over adjectives.** "~40 ms rebuilds", "~7 MB desktop app",
   "18 rules", "100+ tools" — measured, current, and updated when they
   drift. No "blazingly fast".
6. **At most one emoji in the title line**, and only when established
   (Volt ⚡). None in section headings.

## The one-line description

Each project has exactly one canonical description. It appears,
identically or trimmed, in **all** of:

- the GitHub repository description,
- the README first paragraph (may be expanded by one sentence),
- `mix.exs` / `package.json` description (for published packages),
- the org profile README table row.

Shape: *what it is* + *the differentiating clause*. ≤ 140 characters for
the GitHub field. Front-load the noun ("Session recording and replay for
Phoenix LiveView"), not the brand story.

When the description changes, update all four places in the same PR.

## README structure

Canonical section order. Skip sections that don't apply; don't reorder.

```
# Name
<badge row>
<one-paragraph thesis: what + the idea + key facts>
<status blockquote, if not production-ready>

## Quick start | ## Installation     — copy-paste to first success
## Why <Name>                        — the problem, then the inversion
## <capability sections>             — examples first, prose second
## How it works                      — for non-obvious mechanisms
## Limits | ## Status                — honest constraints
## Contributing                      — setup, quality gates, structure
## Part of …                         — ecosystem footer (see below)
## License
```

Rules of thumb:

- **First screen sells, second screen installs.** A reader should know
  whether this is for them without scrolling; if it is, the next thing
  they see is the install command.
- **Show output.** CLI projects include real (trimmed) terminal output.
  Library projects include a real call and its return value. Screenshots
  for anything with a UI.
- **Tables for option/feature matrices**, prose for arguments. Never a
  feature table with a single row of checkmarks against competitors.

## Badge row

Badges sit directly under the H1, one line, in this order. Only badges
that are *true and automated* — no static "build passing" images, no
badge for a workflow that doesn't gate merges.

| Repo type | Badges (in order) |
| --- | --- |
| Hex package | Hex version · Hexdocs · CI |
| npm package | npm version · CI · (bundle size if relevant) |
| Application (e.g. the OpenPencil editor) | CI · latest release · primary package version · distribution channel (Homebrew) · license |
| Infrastructure fork | upstream link in prose instead of badges; CI badge if releases are published |
| Docs / standard / profile repos | no badges |

Templates:

```markdown
[![Hex.pm](https://img.shields.io/hexpm/v/PKG.svg)](https://hex.pm/packages/PKG)
[![Documentation](https://img.shields.io/badge/documentation-gray)](https://hexdocs.pm/PKG)
[![CI](https://github.com/ORG/REPO/actions/workflows/ci.yml/badge.svg)](https://github.com/ORG/REPO/actions/workflows/ci.yml)
```

```markdown
[![npm](https://img.shields.io/npm/v/PKG)](https://www.npmjs.com/package/PKG)
[![CI](https://github.com/ORG/REPO/actions/workflows/ci.yml/badge.svg)](https://github.com/ORG/REPO/actions/workflows/ci.yml)
```

License badges are optional for Hex/npm packages (registries show the
license) and expected for applications. No star counters, no social
badges, no "made with" badges.

## Ecosystem footer

Every repo carries a short footer section linking up. Two tiers:

**Flagships** (volt, vibe, open-pencil, reach, …) get a named section
before License:

```markdown
## Part of <Org>

<Repo> is the <role> of [<Org>](https://github.com/ORG) — <org thesis,
one clause>. The larger picture — why these tools exist and how they
compose — lives in [Building Blocks for the Future
Web](https://github.com/elixir-vibe/building-blocks).
```

**Satellites** (taps, skills, forks, extracted utilities) get one line,
unsectioned, before License:

```markdown
---
Maintained as part of [<Org>](https://github.com/ORG) for
[<flagship>](https://github.com/ORG/REPO).
```

Forks additionally state, in the first paragraph, what they carry on top
of upstream and link the upstream project.

## Org profile READMEs

Each org has a `.github` repo with `profile/README.md` in this shape
(the three existing profiles are the reference implementations):

```
# Org name
<org thesis paragraph — the one idea, stated plainly>
<the "part of a larger stack" paragraph → building-blocks>
<quick start: the single fastest way in>
## Start here          — the one entry-point project
## <category tables>   — project | what it does | registry badge
## How it fits together — ASCII tree of the actual dependency shape
## Why this exists     — the problem, three sentences
```

Org settings that go with it: description matching the org thesis,
website set, six pinned repos (entry point first), social preview image.

## Repository metadata

- **Homepage field**: hexdocs/docs site for packages, product site for
  apps. Never empty when docs exist.
- **Topics**: 5–9, lowercase; include the org theme (`elixir`, `beam`,
  `phoenix` / `design-editor`, `figma-alternative`) plus
  project-specific ones.
- **Releases**: applications publish releases with notes; libraries may
  rely on the registry but tag versions.

## CI

Use the [reusable workflows](README.md) from this repo where they fit,
so the CI badge means the same thing everywhere: `mix ci` on latest
Elixir/OTP plus a minimum-supported run. A repo whose checks differ
documents why in its CONTRIBUTING.

## Checklist for a new (or audited) repo

- [ ] One-line description set in all four places, identical
- [ ] README follows the section order; first screen = thesis + status
- [ ] Badge row per repo type; every badge live and automated
- [ ] Quick start verified by copy-paste into a clean environment
- [ ] Real output/screenshot shown
- [ ] Ecosystem footer (flagship section or satellite line)
- [ ] Homepage field and topics set
- [ ] Listed in the org profile README table
- [ ] Forks: upstream linked, delta stated
