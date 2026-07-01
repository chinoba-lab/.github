# Phase 0 — Security & Governance Checklist

*The first changes to make on the `chinoba-lab` organization, before any
repository is created or transferred.*

> **Scope:** organization **settings and documentation only.** This phase does
> **not** create, transfer, or modify any repository. Run the items here when
> the owner is ready; nothing below is executed automatically.

→ Part of [`IMPLEMENTATION_PLAN.md`](IMPLEMENTATION_PLAN.md) Phase 0.

---

## Pre-flight

- [ ] **The owner's own account has 2FA enabled.** Requiring org 2FA *removes*
      any member/collaborator who lacks it — so enable it on your account first.
- [ ] You can administer the org (`gh api orgs/chinoba-lab` returns admin fields).

---

## 1 · Require 2FA for members

Closes the verified gap (org 2FA was **disabled**).

- **UI:** Org → Settings → *Authentication security* → check **Require
  two-factor authentication for everyone in the `chinoba-lab` organization** →
  Save.
- **Note:** members and outside collaborators without 2FA are removed on save.
  With a single owner today, the only account affected is yours — confirm your
  2FA is on first.
- **Verify:**
  ```
  gh api orgs/chinoba-lab --jq '.two_factor_requirement_enabled'   # → true
  ```

---

## 2 · Restrict repository creation to owners

Makes graduation a deliberate, owner-made decision (today **any member** can
create repos).

- **UI:** Org → Settings → *Member privileges* → **Repository creation** →
  uncheck Public/Private/Internal for members (leave creation to owners).
- **API (review before running):**
  ```
  gh api -X PATCH orgs/chinoba-lab \
    -F members_can_create_repositories=false \
    -F members_can_create_public_repositories=false \
    -F members_can_create_private_repositories=false
  ```
- **Verify:**
  ```
  gh api orgs/chinoba-lab --jq '.members_can_create_repositories'   # → false
  ```

---

## 3 · Set base member permission

Members can read official code but not write by default; write is granted
through team membership (Phase 0 §5).

- **UI:** Org → Settings → *Member privileges* → **Base permissions** → `Read`.
- **API:**
  ```
  gh api -X PATCH orgs/chinoba-lab -f default_repository_permission=read
  ```
- **Verify:**
  ```
  gh api orgs/chinoba-lab --jq '.default_repository_permission'     # → read
  ```

---

## 4 · Owner / member roles

The org-level GitHub roles, mapped to the project roles defined in
[`docs/governance/teams-and-roles.md`](../governance/teams-and-roles.md).

| Org role | Who | May… | Maps to project role |
|---|---|---|---|
| **Owner** | Founder (today: sole owner) | Administer org, settings, create/transfer/archive repos, manage teams & billing, **say no** | Owner |
| **Member** | Trusted contributors (future) | Read all repos; write only where their team grants it; **cannot** create repos | Maintainer / Committer (via team) |
| **Outside collaborator** | Per-repo guests | Access only to explicitly granted repos | Contributor |

- [ ] At least **two owners** *eventually* (avoid a single point of failure) —
      not required while solo, but plan for it before opening the org up.
- [ ] New people join as **Member**, then earn team write — never start as Owner.

> This mirrors the research's Trust Infrastructure theme: access is established,
> traced, and verified — earned through teams, not granted by default.

---

## 5 · Teams (create empty now, populate later)

- [ ] `owners` · `maintainers` · `core` · `interaction` · `execution` ·
      `learning` · `design` · `docs`
- **API (per team):**
  ```
  gh api -X POST orgs/chinoba-lab/teams -f name=core -f privacy=closed
  ```
- The founder is the only member of every team at launch; this lets the second
  maintainer be added with one membership change, not a reorg.

---

## 6 · SECURITY.md guidance

Draft now; **added to the existing `.github` repository in Phase 1**. Phase 1
uses that already-existing repo to publish the organization's Community Health
Files — `SECURITY.md`, `CODEOWNERS`, issue templates, etc. — as the org-wide
defaults that every repo inherits. (Phase 1 does not create the `.github` repo;
it already exists.) Minimum contents:

- [ ] **Supported scope** — which repos/versions receive security fixes.
- [ ] **Private reporting channel** — enable **GitHub Private Vulnerability
      Reporting** at the org level, and/or list `security@chinoba.org`
      (or the org email `hello@chinoba.org`) as the contact.
- [ ] **Response commitment** — acknowledge within N days; handle as a traceable,
      accountable decision (no public issue for unpatched vulnerabilities).
- [ ] **Disclosure policy** — coordinated disclosure; credit reporters.

Draft `SECURITY.md` body:

```markdown
# Security Policy

## Reporting a vulnerability
Please do not open a public issue. Report privately via GitHub's
"Report a vulnerability" on the affected repo, or email security@chinoba.org.

## Scope
Official Chinoba repositories under github.com/chinoba-lab.

## Response
We acknowledge reports within 5 business days and will keep you updated as we
investigate, decide, and remediate. We practice coordinated disclosure and
credit reporters who wish to be named.
```

---

## 7 · Organization governance checklist (baseline)

- [ ] Org profile/contact verified (`hello@chinoba.org`, `chinoba.org`) — *(set ✓)*
- [ ] Two owners planned (single point of failure noted) — §4
- [ ] 2FA required — §1
- [ ] Member repo creation disabled — §2
- [ ] Base permission = Read — §3
- [ ] Teams created — §5
- [ ] SECURITY.md guidance drafted — §6
- [ ] **Branch-protection baseline** documented for official repos:
      protected default branch · ≥1 owning-team review · required status checks ·
      no direct pushes (see [`docs/governance/teams-and-roles.md`](../governance/teams-and-roles.md))
- [ ] Private Vulnerability Reporting enabled at org level
- [ ] Decision-rights matrix acknowledged
      ([`docs/governance/decision-rights.md`](../governance/decision-rights.md))

---

## Verify the whole phase at a glance

```
gh api orgs/chinoba-lab --jq \
  '{twofa: .two_factor_requirement_enabled, \
    member_create: .members_can_create_repositories, \
    base: .default_repository_permission}'
# expected → {"twofa": true, "member_create": false, "base": "read"}

gh api orgs/chinoba-lab/teams --jq '.[].slug'    # the eight teams

# Repository inventory — the `.github` repo already exists; Phase 0 must not
# add any others. Expect exactly one entry.
gh repo list chinoba-lab --json name --jq '.[].name'   # → only: .github
```

---

## Definition of done

- [ ] 2FA required · member repo-creation off · base permission Read
- [ ] Eight teams exist
- [ ] SECURITY.md guidance ready to publish in Phase 1
- [ ] Governance checklist (§7) complete
- [ ] **No *additional* repository created or transferred** — the `.github`
      repo already exists; `gh repo list chinoba-lab` must show only `.github`

→ Next: [`IMPLEMENTATION_PLAN.md`](IMPLEMENTATION_PLAN.md) Phase 1 (org profile + `.github`).

---

← Back to the [chinoba-lab workspace](../../README.md) · [docs/implementation/](README.md)
