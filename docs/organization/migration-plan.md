# Migration Plan

*Organization · Chinoba Ecosystem*

How a repository moves from the personal **Bench** to the official **Commons**
without breaking a single existing link. This is the operational counterpart to
the [graduation gate](repository-strategy.md#the-graduation-gate).

> This is a **plan**, not an executed change. It describes the safe procedure;
> it does not itself touch any repository or GitHub setting.

---

## Principle: transfer, never copy

GitHub's **repository transfer** moves a repo between owners while preserving
its full history, issues, pull requests, releases, and stars — and it installs
an **automatic redirect** from the old URL to the new one.

That single behavior is what makes graduation safe:

> Every `github.com/masao-watanabe-ai/<repo>` URL printed in a book, a blog
> post, or the existing documentation continues to resolve **after** the repo
> moves to `github.com/chinoba-lab/<repo>`.

A fork or re-upload would break that promise (new history, lost stars, dead old
URL). So the rule is absolute: **graduate by transfer.**

### Redirect caveats — what *can* break a redirect

The transfer redirect is durable, but three actions defeat it. Treat them as
prohibited during and after migration:

- **Re-creating a repo of the same name on the personal account.** If
  `masao-watanabe-ai/<repo>` is recreated after the transfer, the redirect is
  shadowed and the old URL stops forwarding. Never recreate a transferred name.
- **Renaming the repo on the org** right after the move. Rename *before*
  transfer if a rename is needed (it isn't — names are already canonical).
- **Package-registry / import paths.** GitHub web redirects do **not** rewrite
  published package coordinates (npm scope, PyPI name, `go get` import path). If
  any repo is published to a registry, plan a version bump that updates the
  source URL; the old published versions keep their old paths.

---

## Pre-flight: before any repo moves

The organization must be ready to receive repos:

1. **Org exists** — `chinoba-lab` is created and verified.
2. **`.github` profile repo exists** — seeded from
   [`../../org-profile/README.md`](../../org-profile/README.md).
3. **Teams exist** — at minimum `maintainers` and `core`
   (→ [`../governance/teams-and-roles.md`](../governance/teams-and-roles.md)).
4. **Community-health defaults exist** — org-level `CODE_OF_CONDUCT`,
   `SECURITY`, `CONTRIBUTING` in `.github`
   (→ [`../standards/community-health.md`](../standards/community-health.md)).

---

## Per-repository procedure

For each repo clearing the graduation gate, in order:

1. **Confirm graduation.** All gate criteria checked and recorded
   (→ [`../governance/decision-rights.md`](../governance/decision-rights.md)).
2. **Bring it to standard on the Bench first.** Add/verify required files
   against [`../standards/repository-template.md`](../standards/repository-template.md)
   *before* the move, so the repo lands clean.
3. **Transfer ownership** `masao-watanabe-ai/<repo>` → `chinoba-lab/<repo>`.
   (GitHub: repo Settings → Danger Zone → Transfer.)
4. **Re-assign ownership.** Add the repo to its layer's team and set
   `CODEOWNERS`.
5. **Apply topics.** Tag with the layer/stream topics from
   [`naming-conventions.md`](naming-conventions.md#topics-not-folders).
6. **Verify the redirect.** Confirm the old URL forwards to the new one.
7. **Record the move.** Note it in the org changelog / repository map.

---

## Sequencing — move in dependency order

Transfer the ecosystem from the center outward, so dependents always find their
dependencies already in place:

```
1. decision-trace-model-v2      (the protocol everything cites)
2. decision-runtime-core         (the kernel)
3. decision-trace-ledger-core-k2 (the record)
4. dtm-view-core-v2              (reads the record)
5. interaction-core-v2          (feeds the kernel)
6. multi-agent-orchestrator-core-v2  (enacts decisions)
7. decision-trace-gnn-core-v2   (learns from the record)
8. decision-trace-studio-v2     (designs the system)
9. light-dtm-starter-kit-cs-v2 / -cs  (entry points — last)
```

Specification and kernel first; consumers and entry points last. The starter
kits move last because they reference the most other repos — by the time they
move, every link they contain already points into the org.

**The website (`chinoba-site`) migrates independently** of this dependency
chain — it has no code dependency on the other repos. It can move early (right
after the `.github` profile is in place), but its transfer carries an extra
step: the **custom domain** `chinoba.org` must follow the repo. If it is served
from GitHub Pages, re-add the custom domain on the org-owned repo and re-verify
DNS; if it is served from an external host, re-point that host's Git integration
at the new owner. Verify chinoba.org resolves before announcing the move.

---

## What does **not** migrate

- **Legacy v1 repos** — frozen and archived on the Bench, never moved
  (→ [`repository-strategy.md`](repository-strategy.md#legacy-policy--the-repos-that-stay)).
- **The personal profile repo** — it describes a person.
- **Sandbox / experiments** — they stay until they graduate or are retired.

---

## After migration: update references (additively)

Once repos live on the org, references in **new** material point to
`chinoba-lab`. The **existing** documentation is left untouched — its old URLs
still resolve via redirect, exactly as designed. No file in the parent
repository is edited as part of this migration.

---

← Back to [Organization](README.md) · [chinoba-lab workspace](../../README.md)
