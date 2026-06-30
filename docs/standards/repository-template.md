# Repository Template

*Standards · Chinoba Ecosystem*

The required shape of an official `chinoba-lab` repository. A repo that matches
this template is ready to graduate; a repo that doesn't, isn't.

---

## Required files

Every official repo carries these, at minimum:

| File | Purpose |
|---|---|
| `README.md` | The repo hub — what it is, its layer, how to use it |
| `LICENSE` | Open-source license (consistent across the org) |
| `CHANGELOG.md` | The stability promise, made visible |
| `CODEOWNERS` | Names the owning team (→ [governance](../governance/teams-and-roles.md)) |
| `CONTRIBUTING.md` | How to contribute (may inherit org default) |
| `.github/` | Issue/PR templates; workflows where relevant |

Community-health files (Code of Conduct, Security policy) may live once at the
org level and be inherited — see [`community-health.md`](community-health.md).

---

## README shape for a code repo

Each repo README opens by **locating itself in the architecture**, so a reader
always knows which layer they're looking at:

```markdown
# <repo-name>

*<Layer> · Chinoba — Intelligence as Relationship*

One sentence: what this repository is and the layer it implements.

> Its place in the flow: …produces signals / decides / executes / learns…

## What it does
## Architecture — where this sits
   → links to neighboring repos and the ecosystem README
## Quick start
## Status & stability
## License

← Part of the Chinoba ecosystem · [chinoba.org](https://chinoba.org)
```

> The first thing a repo README must establish is **its relationship to the
> rest** — never present a Chinoba repo as a standalone tool.

---

## Topics

Apply the standard topics on creation/transfer:

```
chinoba  layer-<x>  stream-<y>  [starter-kit]
```

→ Topic scheme: [`../organization/naming-conventions.md`](../organization/naming-conventions.md#topics-not-folders)

---

## Stability & versioning

- Follow the generation suffix convention (`-v2`, `-k2`) for breaking
  generations (→ [naming](../organization/naming-conventions.md#generation-suffixes)).
- Keep a `CHANGELOG.md`; tag releases.
- State a clear status in the README (e.g. *stable*, *beta*, *spec-draft*).

The Commons makes a promise the Bench does not: that someone can depend on this.
Versioning and a changelog are how that promise is kept honest.

---

## The repo checklist

Before a repo is considered Official:

- [ ] README locates the repo in a layer and links its neighbors
- [ ] LICENSE, CHANGELOG, CODEOWNERS, CONTRIBUTING present
- [ ] Topics applied (`chinoba` + layer + stream)
- [ ] Branch protection on the default branch
- [ ] Owning team assigned (→ governance)
- [ ] Docs follow the [documentation standards](documentation-standards.md)

This checklist *is* the [graduation gate](../organization/repository-strategy.md#the-graduation-gate),
viewed from the repository's side.

---

← Back to [Standards](README.md) · [chinoba-lab workspace](../../README.md)
