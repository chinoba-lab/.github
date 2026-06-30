# Phase 1 — Implementation Guide

**Give the `chinoba-lab` organization a public face: the `.github` repository,
the org profile, and the default community-health files, templates, and
policies every future repo will inherit.**

*Companion to [`IMPLEMENTATION_PLAN.md`](IMPLEMENTATION_PLAN.md) Phase 1. This is
an execution guide — the owner runs the commands when ready. Nothing here is run
automatically.*

---

## Scope & guardrails

| | |
|---|---|
| **In scope** | Create **one** repository: `chinoba-lab/.github`. Add the profile, community-health files, templates, label manifest, and release policy. Define org-wide branch-protection. |
| **Out of scope** | **No Phase 2.** **No repository transfers.** **No other repository is created.** No DNS/hosting change. |
| **Assumes exists** | The **organization only** (`chinoba-lab`). No code repos are assumed to exist. |
| **The one new repo** | `.github` is the sole repository created in Phase 1; it is created fresh, not transferred. |

> Every command targets either the **organization** or the **`.github` repo**.
> No command references any code repository, because none are assumed to exist.

---

## Prerequisites

- [ ] **Phase 0 complete** — 2FA required, member repo-creation off, base
      permission Read, teams created. → [`PHASE_0_SECURITY_CHECKLIST.md`](PHASE_0_SECURITY_CHECKLIST.md)
- [ ] **`gh` authenticated as an org owner:**
      ```
      gh auth status
      gh api orgs/chinoba-lab --jq '.login'        # → chinoba-lab
      ```
- [ ] **Profile source available** — [`profile/README.md`](../../profile/README.md) in this workspace.
- [ ] A working directory to clone into (anywhere outside this docs repo).

---

## 1 · Order of operations

Do them in this order. Step 1 (create `.github`) is the only hard prerequisite
for steps 3–11; steps 2 and 8 are org-level and independent of the repo.

| # | Operation | Section | Depends on |
|---|---|---|---|
| 1 | Confirm/Set org settings (Phase-1-relevant) | §2 | Phase 0 |
| 2 | Create the `.github` repository | §4 | org exists |
| 3 | Add the org profile (`profile/README.md`) | §3, §5 | step 2 |
| 4 | Add community-health files | §6 | step 2 |
| 5 | Add the repository template scaffold | §7 | step 2 |
| 6 | Add labels manifest + issue templates | §9 | step 2 |
| 7 | Add the pull-request template | §10 | step 2 |
| 8 | Add the release-strategy policy | §11 | step 2 |
| 9 | Apply org-wide branch-protection ruleset | §8 | org exists |
| 10 | Push and verify everything renders | §12 | steps 2–8 |

> Steps 3–8 are independent of each other — you can add SECURITY.md without
> labels, or templates without the profile. Only "the `.github` repo must exist
> first" couples them. §12 proves this.

---

## 2 · GitHub Organization settings (Phase-1 relevant)

The **security** settings (2FA, repo-creation, base permission, teams) belong to
**Phase 0** — do not repeat them here; just confirm:

```
gh api orgs/chinoba-lab --jq \
  '{twofa:.two_factor_requirement_enabled, member_create:.members_can_create_repositories, base:.default_repository_permission}'
# expected → {"twofa": true, "member_create": false, "base": "read"}
```

Phase-1 adds these **defaults for how new repos behave** (set once, inherited):

- [ ] **Default branch name = `main`:**
      ```
      gh api -X PATCH orgs/chinoba-lab -f default_repository_branch=main
      ```
- [ ] **Enable org Discussions** (used by SUPPORT + issue-template contact link).
      UI: Org → Settings → Discussions → enable, pick a source repo (`.github`).
- [ ] **Enable Private Vulnerability Reporting org-wide** (pairs with SECURITY.md).
      UI: Org → Settings → Code security → enable for new repositories.
- [ ] **Verify:**
      ```
      gh api orgs/chinoba-lab --jq '.default_repository_branch'   # → main
      ```

**Expected result:** new repos default to `main`, Discussions exist, vulnerability
reporting is on by default.

---

## 3 · Organization profile (what it is)

GitHub renders a special file as the org's public landing page:

> A **public** repo named **`.github`**, with **`profile/README.md`**, becomes
> the page shown at <https://github.com/chinoba-lab>.

That same `.github` repo also holds the **default community-health files** (§6),
**default issue/PR templates** (§9–10), and our **template scaffold** (§7) and
**policies** (§11). One repo, several jobs.

---

## 4 · The `.github` repository

### 4.1 Create it (the only repo created in Phase 1)

```
gh repo create chinoba-lab/.github \
  --public \
  --description "Chinoba Lab — org profile & default community health files"
```

**Expected:** `✓ Created repository chinoba-lab/.github on GitHub`.

**Verify:**
```
gh repo view chinoba-lab/.github --json name,visibility,isEmpty
# → {"name":".github","visibility":"PUBLIC","isEmpty":true}
```

> Must be **public** — a private `.github` repo does not publish the profile or
> default health files.

### 4.2 Clone and lay it out

```
git clone https://github.com/chinoba-lab/.github.git
cd .github
```

Target layout (build this in §3–§11, then push in §12):

```
.github/                         (repo root)
├── profile/
│   └── README.md                ← org profile  (§5)
├── CODE_OF_CONDUCT.md           ← (§6)
├── CONTRIBUTING.md              ← (§6)
├── SECURITY.md                  ← (§6)
├── SUPPORT.md                   ← (§6)
├── RELEASING.md                 ← release policy (§11)
├── FUNDING.yml                  ← optional (§6)
├── labels.yml                   ← canonical label manifest (§9)
├── repo-template/               ← scaffold copied into future repos (§7)
│   ├── README.md
│   ├── LICENSE
│   ├── CHANGELOG.md
│   └── CODEOWNERS
└── .github/                     ← templates (nested .github folder)
    ├── ISSUE_TEMPLATE/
    │   ├── bug_report.yml        ← (§9)
    │   ├── feature_request.yml   ← (§9)
    │   └── config.yml            ← (§9)
    └── PULL_REQUEST_TEMPLATE.md  ← (§10)
```

> Note the nested `.github/` **inside** the `.github` repo — that is the
> canonical location GitHub scans for default issue/PR templates.

---

## 5 · Organization README (`profile/README.md`)

The content already exists in [`profile/README.md`](../../profile/README.md).
Publish it **minus the drafting note**.

```
mkdir -p profile
cp /path/to/chinoba-lab/profile/README.md profile/README.md
```

Then **delete the meta blockquote** near the top (the
`> This is the source for the organization's public profile …` block and its
surrounding `---` separators). Everything else publishes as-is.

**Verify locally:** `profile/README.md` starts with `# Chinoba Lab` and contains
no `> This is the source …` line.

**Expected result after push:** <https://github.com/chinoba-lab> renders the
profile.

---

## 6 · Community Health files

These live at the **root** of the `.github` repo and are inherited by any org
repo that lacks its own. Copy each block verbatim.

### 6.1 `CODE_OF_CONDUCT.md`
```markdown
# Code of Conduct

Chinoba is built on the premise that intelligence is relational. This project
depends on healthy relationships, so we hold contributors to the
[Contributor Covenant v2.1](https://www.contributor-covenant.org/version/2/1/code_of_conduct/).

## Our pledge
We pledge to make participation a harassment-free experience for everyone.

## Enforcement
Report unacceptable behavior to **hello@chinoba.org**. Maintainers will review
and respond, and may remove, edit, or reject contributions or contributors that
violate this Code of Conduct.
```

### 6.2 `CONTRIBUTING.md`
```markdown
# Contributing to Chinoba

Thank you for helping build the infrastructure for Human–AI Coordination.

In Chinoba's terms: your contribution is a **signal**; a maintainer reviews it
within a repo's **boundaries**; a human **decides**; the merge and its
discussion are the **trace**.

## Before you start
- For anything non-trivial, **open an issue first** so we can agree on scope.
- Small fixes (typos, docs) can go straight to a pull request.

## Workflow
1. Fork or branch.
2. Make your change; follow the repo's standards
   (README, CHANGELOG, tests where applicable).
3. Open a pull request using the template.
4. The owning team (via CODEOWNERS) reviews.
5. A maintainer merges — or declines, with a reason.

## Standards
- Documentation style: the Chinoba documentation standards.
- Repository layout: the repository template.
- Be kind; see our Code of Conduct.

Maintainers may decline any change that does not fit the philosophy or
standards. That is the project's "right to say no," and it keeps the Commons
trustworthy.
```

### 6.3 `SECURITY.md`
*(identical to the draft prepared in [`PHASE_0_SECURITY_CHECKLIST.md`](PHASE_0_SECURITY_CHECKLIST.md) §6 — keep them in sync.)*
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

### 6.4 `SUPPORT.md`
```markdown
# Support

- **Questions & ideas** → [GitHub Discussions](https://github.com/orgs/chinoba-lab/discussions)
- **Bugs** → open an issue in the relevant repository
- **Security** → see SECURITY.md (do not file public issues)
- **Learn more** → [chinoba.org](https://chinoba.org) ·
  [research blog](https://deus-ex-machina-ism.com) ·
  [library](https://chinoba.org/library/)

Support is best-effort. Official repositories carry a stability promise;
experiments on the founder's personal account do not.
```

### 6.5 `FUNDING.yml` (optional)
```yaml
# Uncomment and fill if/when sponsorship is set up.
# github: [chinoba-lab]
# custom: ["https://chinoba.org"]
```

**Verify (§12 covers the live check):** all five files exist at repo root.

---

## 7 · Repository template

Two layers, both satisfied without creating any extra repository:

1. **The standard** is already documented:
   [`docs/standards/repository-template.md`](../standards/repository-template.md).
2. **A ready-to-copy scaffold** lives in `.github/repo-template/`, so future
   official repos start compliant. Build it now:

`repo-template/README.md`
```markdown
# <repo-name>

*<Layer> · Chinoba — Intelligence as Relationship*

One sentence: what this repository is and the layer it implements.

> Its place in the flow: …produces signals / decides / executes / learns…

## What it does
## Architecture — where this sits
## Quick start
## Status & stability
## License

← Part of the Chinoba ecosystem · [chinoba.org](https://chinoba.org)
```

`repo-template/CHANGELOG.md`
```markdown
# Changelog
All notable changes are documented here. Format: Keep a Changelog; versioning: SemVer.

## [Unreleased]
```

`repo-template/CODEOWNERS`
```
# Replace @chinoba-lab/<team> with the owning layer team.
*        @chinoba-lab/<team>
/docs/   @chinoba-lab/docs
```

`repo-template/LICENSE` — add the org's chosen OSS license text (decide once;
apply to all official repos).

> **Optional, later:** mark a future repo as a GitHub *template repository* so
> "Use this template" works in the UI. Not required in Phase 1, and it is **not**
> created here (no extra repos in Phase 1).

**Expected result:** a self-contained scaffold under `repo-template/` that a
maintainer copies when a repo graduates in a later phase.

---

## 8 · Branch protection recommendations

Use an **organization-wide ruleset** — it can be created **before** code repos
exist and applies to them automatically when they do. This is independent of the
`.github` repo.

### 8.1 Org ruleset (recommended)

UI path: Org → Settings → *Repository rulesets* → New ruleset → Branch ruleset →
target **All repositories**, branch **Default branch**, and enable:
require a pull request before merging (≥1 approval, require review from Code
Owners), block force pushes, restrict deletions.

API equivalent (review before running):
```
gh api -X POST orgs/chinoba-lab/rulesets --input - <<'JSON'
{
  "name": "default-branch-protection",
  "target": "branch",
  "enforcement": "active",
  "conditions": {
    "ref_name": { "include": ["~DEFAULT_BRANCH"], "exclude": [] },
    "repository_name": { "include": ["~ALL"], "exclude": [] }
  },
  "rules": [
    { "type": "deletion" },
    { "type": "non_fast_forward" },
    { "type": "pull_request",
      "parameters": {
        "required_approving_review_count": 1,
        "require_code_owner_review": true,
        "dismiss_stale_reviews_on_push": true,
        "require_last_push_approval": false,
        "required_review_thread_resolution": false
      }
    }
  ],
  "bypass_actors": [
    { "actor_type": "OrganizationAdmin", "bypass_mode": "always" }
  ]
}
JSON
```

> **Deliberately omitted now:** `required_status_checks`. No CI checks exist yet
> (no code repos). Adding a required check that never runs would block every
> merge. Add status checks **per repo** when CI is introduced (later phases).

### 8.2 Solo-maintainer reality

With one owner, "require 1 approval + code-owner review" cannot be satisfied by
yourself (GitHub blocks self-approval). Two honest options:

- **Keep the rule on**, and rely on the **OrganizationAdmin bypass** above while
  solo. When a second maintainer joins, **remove the bypass** so the rule binds.
- **Or** set `required_approving_review_count` to `0` until a second maintainer
  exists, then raise it.

This is the Governance theme's "right to say no" with a pragmatic bypass for the
single-owner stage — documented, not hidden.

### 8.3 Verify
```
gh api orgs/chinoba-lab/rulesets --jq '.[].name'   # → default-branch-protection
```

---

## 9 · Labels and issue templates

### 9.1 Labels — a canonical manifest

GitHub does **not** auto-apply org-wide labels. Define them once in
`labels.yml`; apply per repo when repos are created (later phases). In Phase 1
you may apply them to `.github` itself to dogfood.

`labels.yml`
```yaml
# type
- { name: "type:bug",         color: "d73a4a", description: "Something is broken" }
- { name: "type:feature",     color: "0e8a16", description: "New capability" }
- { name: "type:docs",        color: "0075ca", description: "Documentation" }
- { name: "type:question",    color: "d876e3", description: "A question" }
# architecture
- { name: "layer:interaction",color: "fbca04", description: "Interaction layer" }
- { name: "layer:core",       color: "5319e7", description: "Decision Runtime / core" }
- { name: "layer:execution",  color: "1d76db", description: "Execution layer" }
- { name: "layer:learning",   color: "0e8a16", description: "Learning layer" }
- { name: "layer:design",     color: "c5def5", description: "Design layer" }
- { name: "stream:decision",  color: "b60205", description: "Decision Stream" }
- { name: "stream:coordination", color: "006b75", description: "Coordination Stream" }
# workflow
- { name: "priority:high",    color: "b60205", description: "Urgent" }
- { name: "status:blocked",   color: "e99695", description: "Blocked on something" }
- { name: "good first issue", color: "7057ff", description: "Good entry point" }
- { name: "help wanted",      color: "008672", description: "Maintainers want help" }
```

Apply to a repo (example, **for the `.github` repo only** in Phase 1):
```
# requires: gh extension or a loop; simplest per-label:
gh label create "type:bug" --repo chinoba-lab/.github --color d73a4a --description "Something is broken"
# …repeat per label, or script a loop over labels.yml
```

> Applying the manifest to **code repos** happens in their migration phase — not
> Phase 1 (no code repos exist to label).

### 9.2 Issue templates (default, inherited org-wide)

Place under `.github/ISSUE_TEMPLATE/` inside the `.github` repo.

`.github/ISSUE_TEMPLATE/bug_report.yml`
```yaml
name: Bug report
description: Report something that is broken in an official Chinoba repo
labels: ["type:bug"]
body:
  - type: textarea
    id: what
    attributes: { label: What happened?, description: Expected vs actual }
    validations: { required: true }
  - type: textarea
    id: repro
    attributes: { label: Steps to reproduce }
    validations: { required: true }
  - type: input
    id: version
    attributes: { label: Version / commit }
```

`.github/ISSUE_TEMPLATE/feature_request.yml`
```yaml
name: Feature request
description: Propose a capability or change
labels: ["type:feature"]
body:
  - type: textarea
    id: problem
    attributes: { label: The problem, description: What relationship or decision is hard today? }
    validations: { required: true }
  - type: textarea
    id: proposal
    attributes: { label: Proposed direction }
```

`.github/ISSUE_TEMPLATE/config.yml`
```yaml
blank_issues_enabled: false
contact_links:
  - name: Questions & discussion
    url: https://github.com/orgs/chinoba-lab/discussions
    about: Ask questions and discuss ideas here.
  - name: Chinoba website
    url: https://chinoba.org
    about: Background, research, and books.
```

**Expected result:** repos without their own templates show these when opening an
issue.

---

## 10 · Pull request template

`.github/PULL_REQUEST_TEMPLATE.md` inside the `.github` repo:

```markdown
## What & why
<!-- What does this change, and what relationship/decision does it serve? -->

## Type
- [ ] Bug fix
- [ ] Feature
- [ ] Docs
- [ ] Refactor / chore

## Checklist
- [ ] Linked to an issue (for non-trivial changes)
- [ ] Follows the repository standards (README/CHANGELOG updated as needed)
- [ ] Tests pass / not applicable
- [ ] Ready for owning-team (CODEOWNERS) review

## Notes for reviewers
<!-- Anything that helps the human who decides. -->
```

**Expected result:** new PRs in repos without their own template are pre-filled
with this.

---

## 11 · Release strategy

Define the policy now (`RELEASING.md`); apply it when code repos exist. The
`.github` repo itself is docs-only and is **not released**.

`RELEASING.md`
```markdown
# Release Strategy

## Versioning
- Official repos use **SemVer** (`MAJOR.MINOR.PATCH`), tagged `vX.Y.Z`.
- A breaking architectural **generation** is a new repo with a suffix
  (`-v2`, `-k2`), not an in-place rewrite — the old repo stays for history.

## Releasing
1. Update `CHANGELOG.md` (Keep a Changelog format).
2. Tag `vX.Y.Z` on the default branch.
3. Publish a GitHub Release with notes derived from the changelog.

## Stability
- The Commons carries a stability promise; the personal Bench does not.
- State each repo's status (stable / beta / spec-draft) in its README.

## Packages
- If a repo publishes to a registry (npm/PyPI/…), a version bump must accompany
  any source-URL change after a transfer (registry paths do not auto-redirect).
```

**Expected result:** a single, referenceable release policy for all future
official repos.

---

## 12 · Independence verification

Confirm each Phase-1 deliverable stands on its own. The only shared prerequisite
is "the `.github` repo exists" (steps 3–8); steps 2 and 8 are org-level.

| Deliverable | Independent of | Sole prerequisite | Standalone check |
|---|---|---|---|
| Org settings (§2) | everything | org exists | `gh api orgs/chinoba-lab` |
| `.github` repo (§4) | everything | org exists | `gh repo view chinoba-lab/.github` |
| Profile (§5) | health/templates/labels | `.github` exists | profile URL renders |
| Community-health (§6) | profile/templates | `.github` exists | files at repo root resolve |
| Repo template (§7) | all others | `.github` exists | `repo-template/` present |
| Labels (§9.1) | templates/profile | `.github` exists | `gh label list` |
| Issue templates (§9.2) | profile/labels | `.github` exists | "New issue" shows forms |
| PR template (§10) | all others | `.github` exists | new PR pre-filled |
| Release policy (§11) | all others | `.github` exists | `RELEASING.md` present |
| Branch ruleset (§8) | `.github` contents | org exists | `gh api …/rulesets` |

> **Result:** no deliverable depends on a code repository, on Phase 2, or on any
> repository transfer. Each can be completed and verified on its own.

### Full verification run (after §13 push)

```
# Profile renders
open https://github.com/chinoba-lab
# Health files resolve (community profile)
gh api repos/chinoba-lab/.github/community/profile --jq '.files | keys'
# Key files present
for f in profile/README.md CODE_OF_CONDUCT.md CONTRIBUTING.md SECURITY.md \
         SUPPORT.md RELEASING.md labels.yml .github/PULL_REQUEST_TEMPLATE.md; do
  gh api repos/chinoba-lab/.github/contents/$f --jq '.path' 2>/dev/null || echo "MISSING: $f"
done
# Ruleset active
gh api orgs/chinoba-lab/rulesets --jq '.[].name'
```

---

## 13 · Commit & push

```
git add -A
git commit -m "Phase 1: org profile, community health, templates, policies"
git push origin main
```

**Expected:** push succeeds. (If the org ruleset is already active with required
reviews, push to `main` directly using your OrganizationAdmin bypass, or open the
first PR — see §8.2.)

---

## Definition of Done

- [ ] `chinoba-lab/.github` exists and is **public**
- [ ] <https://github.com/chinoba-lab> renders the profile
- [ ] CODE_OF_CONDUCT, CONTRIBUTING, SECURITY, SUPPORT resolve as org defaults
- [ ] Issue templates + config + PR template in place
- [ ] `repo-template/` scaffold present
- [ ] `labels.yml` + `RELEASING.md` present
- [ ] Org branch-protection ruleset active
- [ ] §12 independence table all green
- [ ] **No repository transferred; no repo created except `.github`** — confirmed

---

> **Stop here. Do not proceed to Phase 2 (website ownership migration).**
> Phase 2 begins only on explicit instruction.

← Back to the [chinoba-lab workspace](../../README.md) · [docs/implementation/](README.md) · [IMPLEMENTATION_PLAN.md](IMPLEMENTATION_PLAN.md)
