# Organization Security & Governance — Execution Plan (for review)

**Every remaining task that lives in GitHub *Organization settings* (not repository
files), grouped and specified for review. Repository-side Phase 1 is done; this
plan covers the org-level work only.**

> **This is a review artifact. Nothing here has been executed.**
> No org setting has been changed and no GitHub write operation has been run.
> Each item states whether it is UI-only or `gh`/API-capable, the exact command,
> a verification command, the expected result, and a risk level.

*Consolidates the org-level items from [`PHASE_0_SECURITY_CHECKLIST.md`](PHASE_0_SECURITY_CHECKLIST.md)
and the org-level sections of [`PHASE_1_IMPLEMENTATION_GUIDE.md`](PHASE_1_IMPLEMENTATION_GUIDE.md)
(§2 defaults, §8 ruleset), and adds the security surfaces those guides do not yet
cover (Actions permissions, Dependabot / Code / Secret scanning defaults).*

---

## 0 · Read before running anything

### 0.1 Plan-tier reality (affects several items below)

`chinoba-lab` is a **free** organization. That constrains what is available:

| Capability | Free org | Consequence for this plan |
|---|---|---|
| Dependabot alerts / security updates / dependency graph | ✅ all repos | Enable freely |
| Secret scanning + push protection | ✅ **public** repos only | Works for our public repos; private repos would need paid GHAS |
| Code scanning (CodeQL default setup) | ✅ **public** repos only | Works for public repos; private repos need paid GHAS |
| Rulesets / branch protection | ✅ **public** repos; ❌ private repos on free | Our official repos are public → fine |
| `Internal` repo visibility | ❌ | Use `private`; not an org-settings task |
| Actions permissions & default token scope | ✅ | Enable freely |

> **Net:** because Chinoba's official repos are **public**, every security feature
> in this plan is available on the free plan. The only blocked surfaces
> (Advanced Security on *private* repos) are out of scope.

### 0.2 Prerequisites

- `gh` authenticated as an **org owner** with `admin:org` scope
  (verified: token has `admin:org`, `repo`, `workflow`).
- The **owner's own account has 2FA enabled** *before* running §1.1 (enabling the
  org 2FA requirement removes any member lacking it).

### 0.3 Baseline audit — run this first (READ-ONLY, safe)

Establishes what is already set so you only apply the deltas:

```bash
gh api orgs/chinoba-lab --jq '{
  twofa:                .two_factor_requirement_enabled,
  member_create:        .members_can_create_repositories,
  member_create_public: .members_can_create_public_repositories,
  member_create_private:.members_can_create_private_repositories,
  base:                 .default_repository_permission,
  default_branch:       .default_repository_branch,
  dependabot_alerts_new:.dependabot_alerts_enabled_for_new_repositories,
  dep_graph_new:        .dependency_graph_enabled_for_new_repositories,
  secret_scan_new:      .secret_scanning_enabled_for_new_repositories,
  secret_push_new:      .secret_scanning_push_protection_enabled_for_new_repositories
}'
gh api orgs/chinoba-lab/teams --jq '.[].slug'
gh api orgs/chinoba-lab/rulesets --jq '.[].name'
gh api orgs/chinoba-lab/actions/permissions
gh api orgs/chinoba-lab/actions/permissions/workflow
gh repo list chinoba-lab --json name --jq '.[].name'   # expect only: .github
```

### 0.4 Legend

- **Method:** `UI` = must be done in the GitHub web UI · `API` = doable via `gh`/REST · `Both` = either.
- **Risk:** **Low** = reversible, no blast radius · **Medium** = can block a workflow/merge/push · **High** = can remove access or lock the org out.
- ⚠ = accuracy caveat — verify the field/endpoint against `gh api` before relying on it.
- ✅ = **verified already in place** by the 2026-07-01 baseline audit — no action needed.

### 0.5 Audit status — verified 2026-07-01 (read-only)

The baseline audit found the org already hardened well beyond the original
assumptions. Items marked ✅ below need **no action**.

| Area | State |
|---|---|
| **Phase A** defaults — repo-creation owners-only, base=`read`, default branch `main`, private-fork off | ✅ done |
| **2FA** requirement (§1.1) | ✅ done |
| **Eight teams** (§9 / Phase B) | ✅ done |
| Read-only workflow token (§7.2) | ✅ done |
| **New-repo security defaults** (§5.2) | ✅ done (2026-07-01) |
| **Actions allowlist** (§7.1) | ✅ done (2026-07-01) |
| Org-wide **rulesets** (§3) | ⛔ **blocked** — free plan returns HTTP 403 |
| Per-repo branch protection for `.github` (§4) | ⬜ optional — next action |

> **Next actionable item** (Phase R, minus the blocked ruleset):
> **§4** (optional) per-repo classic branch protection for `.github`.

---

## 1 · Organization Security

| # | Item | Method | Risk |
|---|---|---|---|
| 1.1 | Require 2FA for everyone | **UI only** ⚠ | **High** |
| 1.2 | Plan for ≥2 owners (avoid single point of failure) | Governance/manual | Low |
| 1.3 | Verify org profile contact (`hello@chinoba.org`, `chinoba.org`) | Both | Low |

**1.1 — Require two-factor authentication** — ✅ **already enabled** (audit 2026-07-01: `two_factor_requirement_enabled = true`). No action needed.
The 2FA *requirement* cannot
be toggled through the org PATCH API — `two_factor_requirement_enabled` is
**read-only**; this must be done in the UI. ⚠

- **UI:** Org → Settings → *Authentication security* → check **Require two-factor
  authentication for everyone in `chinoba-lab`** → Save.
- **Verify:** `gh api orgs/chinoba-lab --jq '.two_factor_requirement_enabled'`
- **Expected:** `true`
- **Risk — High:** on Save, any member/outside collaborator **without 2FA is
  removed**. Mitigation: solo org today → only your own account is affected, so
  confirm your 2FA first (0.2).

**1.2 — Two-owner plan** — no command; add a second owner before opening the org.
Verify eventual state: `gh api orgs/chinoba-lab/members --jq '[.[]|.login]'` and
role via `gh api orgs/chinoba-lab/memberships/<user> --jq '.role'`. **Risk: Low.**

**1.3 — Contact/profile** — `gh api orgs/chinoba-lab --jq '{email:.email, blog:.blog, name:.name}'`. Expected: `hello@chinoba.org`, `chinoba.org`, `Chinoba`. **Risk: Low.**

---

## 2 · Repository Defaults (how new repos behave)

| # | Item | Method | Risk |
|---|---|---|---|
| 2.1 | Base member permission = `read` — ✅ done | **API** | Low |
| 2.2 | Default branch name = `main` for new repos — ✅ done | **API/UI** | Low |
| 2.3 | **(Optional)** Enable org Discussions | **UI only** | Low |

**2.1 — Base permission Read** — ✅ **already set** (audit: `default_repository_permission = read`).
(members read official code, write only via team)

- **API:** `gh api -X PATCH orgs/chinoba-lab -f default_repository_permission=read`
- **UI:** Org → Settings → *Member privileges* → **Base permissions** → `Read`.
- **Verify:** `gh api orgs/chinoba-lab --jq '.default_repository_permission'`
- **Expected:** `read` · **Risk: Low.**

**2.2 — Default branch `main`** — ✅ **already set** (audit: `default_repository_branch = main`).
The 2026-07-01 audit confirmed the `default_repository_branch` field **does** read
back reliably (earlier caveat resolved); the API form below is safe if ever needed:
`gh api -X PATCH orgs/chinoba-lab -f default_repository_branch=main`.

- **UI:** Org → Settings → *Repository* → **Default branch name** → `main` → Update.
- **Verify (UI-confirmable; API read may be absent ⚠):** create-time behavior, or
  `gh api orgs/chinoba-lab --jq '.default_repository_branch'`
- **Expected:** `main` · **Risk: Low** (affects only *new* repos).

**2.3 — Discussions** (used by SUPPORT.md + issue-template contact link)

> **Optional — not part of Phase A.** Enable only when you actually want to open
> a discussion surface; the SUPPORT.md link is harmless until then.

- **UI only:** Org → Settings → *Discussions* → enable, source repo `.github`.
- **Verify:** open `https://github.com/orgs/chinoba-lab/discussions`.
- **Expected:** discussions tab renders · **Risk: Low.**

---

## 3 · Rulesets (preferred over classic branch protection)

> ⛔ **BLOCKED on the current GitHub Free plan.** The 2026-07-01 audit ran
> `GET /orgs/chinoba-lab/rulesets` and received **HTTP 403 — "Upgrade to GitHub
> Team to enable this feature."** Org-wide rulesets are **not** available on Free
> (this is stronger than the earlier "public repos only" caveat — the feature is
> gated entirely). **Therefore §3 must be deferred until a GitHub Team upgrade, or
> replaced with classic per-repo branch protection (§4).** For `.github` today,
> use §4.

| # | Item | Method | Risk |
|---|---|---|---|
| 3.1 | Org-wide default-branch ruleset (all repos) — ⛔ blocked (Free plan) | **API** (UI equivalent) | Medium |
| 3.2 | Solo-maintainer reconciliation (bypass vs. 0 approvals) — ⛔ blocked (Free plan) | Decision + API | Medium |

**3.1 — Org ruleset**, created once, auto-applies to repos as they appear.
Targets **all repositories**, **default branch**; requires PR + code-owner review,
blocks force-push, restricts deletion. `required_status_checks` **deliberately
omitted** (no CI exists; a never-running required check would block every merge).

- **API (review the JSON, then run):**
  ```bash
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
- **UI equivalent:** Org → Settings → *Repository rulesets* → New → Branch ruleset →
  target All repositories / Default branch → enable require-PR (1 approval, code-owner
  review), block force pushes, restrict deletions.
- **Verify:** `gh api orgs/chinoba-lab/rulesets --jq '.[].name'`
- **Expected:** `default-branch-protection`
- **Risk — Medium:** active enforcement blocks direct pushes and unreviewed merges
  org-wide. The `OrganizationAdmin` bypass keeps a solo owner unblocked (see 3.2).
  On free plan this binds **public** repos only.

**3.2 — Solo-maintainer reconciliation** (a decision, not a new setting)
GitHub blocks self-approval, so "1 approval + code-owner review" can't be met solo.
Pick one:
- **(Recommended)** Keep the rule on and rely on the `OrganizationAdmin` bypass
  above while solo; **remove the bypass** when a second maintainer joins.
- **Or** set `required_approving_review_count: 0` now, raise to `1` later.

Change approval count later by capturing the ruleset id and PATCHing:
```bash
RID=$(gh api orgs/chinoba-lab/rulesets --jq '.[]|select(.name=="default-branch-protection").id')
gh api orgs/chinoba-lab/rulesets/$RID --jq '.rules'   # inspect before editing
```
**Risk — Medium** (mis-set count can block or under-protect merges).

---

## 4 · Branch Protection (classic — alternative / per-repo)

| # | Item | Method | Risk |
|---|---|---|---|
| 4.1 | Classic branch protection on a specific repo | **API** (UI) | Medium |

Rulesets (§3) are the org-wide mechanism and are **preferred**. Classic branch
protection is **per repository** and only relevant if you want protection on the
`.github` repo itself before/instead of the org ruleset, or need a check the
ruleset doesn't express. Do **not** run both redundantly.

- **API (example, `.github` repo default branch):**
  ```bash
  gh api -X PUT repos/chinoba-lab/.github/branches/main/protection --input - <<'JSON'
  {
    "required_status_checks": null,
    "enforce_admins": false,
    "required_pull_request_reviews": {
      "required_approving_review_count": 1,
      "require_code_owner_reviews": true,
      "dismiss_stale_reviews": true
    },
    "restrictions": null,
    "allow_force_pushes": false,
    "allow_deletions": false
  }
  JSON
  ```
- **UI:** Repo → Settings → *Branches* → Add branch protection rule.
- **Verify:** `gh api repos/chinoba-lab/.github/branches/main/protection --jq '.required_pull_request_reviews'`
- **Expected:** review block present, `allow_force_pushes.enabled=false`.
- **Risk — Medium:** with solo owner and `enforce_admins:false`, you can still push;
  set `enforce_admins:true` only when a second maintainer exists.

> **Recommendation:** rely on the org ruleset (§3); skip §4 unless a per-repo need
> arises. Listed for completeness per the review request.

---

## 5 · Security Features (org-level defaults for *new* repos)

| # | Item | Method | Risk |
|---|---|---|---|
| 5.1 | Private Vulnerability Reporting, org-wide | **API** (UI) | Low |
| 5.2 | "Enable for new repositories" security defaults — ✅ done (2026-07-01) | **Both** ⚠ | Low |

**5.1 — Private Vulnerability Reporting (PVR)** — pairs with SECURITY.md.

> **Timing — not Phase A.** Enable when the organization actually contains
> **production code repositories** to receive reports against. Until then the
> `.github` docs repo has nothing to report against, so this is deferred.

- **API (enable for the org / all repos):** `gh api -X PUT orgs/chinoba-lab/private-vulnerability-reporting`
  (disable = `-X DELETE`). ⚠ confirm endpoint on your `gh`/API version.
- **UI:** Org → Settings → *Code security* → **Private vulnerability reporting** →
  enable for new repositories (and "Enable all" for existing).
- **Verify (per repo):** `gh api repos/chinoba-lab/.github/private-vulnerability-reporting --jq '.enabled'`
- **Expected:** `true` · **Risk: Low.**

**5.2 — Security defaults for new repos** — ✅ **DONE (executed 2026-07-01).**
All five new-repository defaults now read `true`: dependency graph, Dependabot
alerts, Dependabot security updates, secret scanning, and secret-scanning push
protection. Applied via the org PATCH form below (returned HTTP 200; verification
confirmed all five). Affects repos created from now on; no existing repo touched.

- **API (org PATCH form):**
  ```bash
  gh api -X PATCH orgs/chinoba-lab \
    -F dependency_graph_enabled_for_new_repositories=true \
    -F dependabot_alerts_enabled_for_new_repositories=true \
    -F dependabot_security_updates_enabled_for_new_repositories=true \
    -F secret_scanning_enabled_for_new_repositories=true \
    -F secret_scanning_push_protection_enabled_for_new_repositories=true
  ```
- **UI (recommended, always current):** Org → Settings → *Code security* →
  toggle each "…for new repositories," or create/apply a Code security configuration.
- **Verify:** re-run the security fields from the 0.3 baseline audit.
- **Expected:** the toggled fields read `true`.
- **Risk — Low** (affects only *new* repos; existing repos are handled in §6).

---

## 6 · Dependabot / Code Scanning / Secret Scanning (existing repos)

| # | Item | Method | Risk |
|---|---|---|---|
| 6.1 | Dependency graph — enable all existing repos | **API** (UI) | Low |
| 6.2 | Dependabot alerts — enable all | **API** (UI) | Low |
| 6.3 | Dependabot security updates — enable all | **API** (UI) | Low |
| 6.4 | Secret scanning — enable all (public) | **API** (UI) | Low |
| 6.5 | Secret scanning **push protection** — enable all | **API** (UI) | **Medium** |
| 6.6 | Code scanning (CodeQL default setup) | **API/UI, per repo** | Low |

**6.1–6.5 — Bulk enable across all current repos** (today only `.github` exists).
Uses `POST /orgs/{org}/{security_product}/{enablement}`:

```bash
for prod in dependency_graph dependabot_alerts dependabot_security_updates \
            secret_scanning secret_scanning_push_protection; do
  gh api -X POST "orgs/chinoba-lab/$prod/enable_all"
done
```
- **UI equivalent:** Org → Settings → *Code security* → each feature → **Enable all**.
- **Verify (per repo):** `gh api repos/chinoba-lab/.github --jq '.security_and_analysis'`
- **Expected:** each product shows `"status":"enabled"`.
- **Risk:** 6.1–6.4 **Low**. **6.5 push protection = Medium** — it *rejects pushes
  containing detected secrets*; benign for a docs repo but can surprise contributors
  (bypassable with a reason). Free plan: secret scanning is **public-repo only**.

**6.6 — Code scanning (CodeQL) default setup** — for *code* repos, not `.github`.
Free plan: **public repos only**. No code repos exist yet, so this is a
**per-repo step for Phase 3 migrations**, listed here for completeness.

- **API (per repo, when it exists):**
  `gh api -X PUT repos/chinoba-lab/<repo>/code-scanning/default-setup -f state=configured`
- **UI:** Repo → Settings → *Code security* → Code scanning → **Set up → Default**.
- **Verify:** `gh api repos/chinoba-lab/<repo>/code-scanning/default-setup --jq '.state'`
- **Expected:** `configured` · **Risk: Low** (adds a workflow; runs on push/PR).

> Not in the current Phase 0/1 guides — **recommended addition**. `.github` is
> docs-only, so code scanning applies from Phase 3 onward.

---

## 7 · Actions Permissions

| # | Item | Method | Risk |
|---|---|---|---|
| 7.1 | Restrict which Actions can run — ✅ done (2026-07-01) | **API** (UI) | Medium |
| 7.2 | Default `GITHUB_TOKEN` = read-only — ✅ done | **API** (UI) | Low–Medium |
| 7.3 | Require approval for fork-PR workflows | **UI** ⚠ | Low |

> **Not covered by the current guides — recommended additions.** Sensible even
> with no code repos yet, since the policy is inherited by future repos.

**7.1 — Allowed actions** — ✅ **DONE (executed 2026-07-01).** Least-privilege:
GitHub-owned + Marketplace verified-creator actions only. Verified final state:
`enabled_repositories=all`, `allowed_actions=selected`, `github_owned_allowed=true`,
`verified_allowed=true`, `patterns_allowed=[]`. Both PUTs returned success.

- **API (as executed — two steps; Step A must precede Step B, else 409):**
  ```bash
  gh api -X PUT orgs/chinoba-lab/actions/permissions \
    -F enabled_repositories=all -f allowed_actions=selected
  gh api -X PUT orgs/chinoba-lab/actions/permissions/selected-actions --input - <<'JSON'
  { "github_owned_allowed": true, "verified_allowed": true, "patterns_allowed": [] }
  JSON
  ```
  > Use the `--input` JSON body for `patterns_allowed`; `gh api -F patterns_allowed='[]'`
  > sends the literal string `"[]"` and is rejected.
- **UI:** Org → Settings → *Actions → General* → **Allow select actions** → tick
  "Allow actions created by GitHub" + "…by verified creators."
- **Verify:**
  ```bash
  gh api orgs/chinoba-lab/actions/permissions --jq '{enabled:.enabled_repositories, allowed:.allowed_actions}'
  gh api orgs/chinoba-lab/actions/permissions/selected-actions
  ```
- **Rollback (revert to permissive):**
  `gh api -X PUT orgs/chinoba-lab/actions/permissions -F enabled_repositories=all -f allowed_actions=all`
- **Risk — Medium:** an over-tight allowlist can break future workflows; none exist
  today. Add third-party non-verified actions to `patterns_allowed` per real need.

**7.2 — Default workflow token permissions = read-only** — ✅ **already set**
(audit: `default_workflow_permissions = read`, `can_approve_pull_request_reviews = false`). (no auto write, no PR approval)

- **API:**
  ```bash
  gh api -X PUT orgs/chinoba-lab/actions/permissions/workflow \
    -F default_workflow_permissions=read \
    -F can_approve_pull_request_reviews=false
  ```
- **UI:** Org → Settings → *Actions → General* → **Workflow permissions** →
  "Read repository contents…" + untick "Allow GitHub Actions to approve PRs."
- **Verify:** `gh api orgs/chinoba-lab/actions/permissions/workflow`
- **Expected:** `{"default_workflow_permissions":"read","can_approve_pull_request_reviews":false}`
- **Risk — Low–Medium:** workflows needing write must opt in per-job via `permissions:`
  — the secure default; may require touching a future workflow file.

**7.3 — Fork-PR approval** (require approval before running workflows from forks)

- **UI only ⚠:** Org → Settings → *Actions → General* → *Fork pull request
  workflows from outside collaborators* → **Require approval for all outside
  collaborators** (no stable org-level API field). 
- **Verify:** UI state (no reliable read endpoint).
- **Risk — Low** (prevents drive-by CI abuse; only matters once public repos take PRs).

---

## 8 · Repository Creation Policies

| # | Item | Method | Risk |
|---|---|---|---|
| 8.1 | Restrict repo creation to owners — ✅ done | **API** | Low |
| 8.2 | (Optional) forking / member privileges — ✅ done | Both | Low |

**8.1 — Members cannot create repositories** — ✅ **already set** (audit: all three
`members_can_create_*` fields = `false`). (keeps graduation an owner decision)

- **API:**
  ```bash
  gh api -X PATCH orgs/chinoba-lab \
    -F members_can_create_repositories=false \
    -F members_can_create_public_repositories=false \
    -F members_can_create_private_repositories=false
  ```
- **UI:** Org → Settings → *Member privileges* → **Repository creation** →
  uncheck all member options.
- **Verify:** `gh api orgs/chinoba-lab --jq '{r:.members_can_create_repositories, pub:.members_can_create_public_repositories, priv:.members_can_create_private_repositories}'`
- **Expected:** all `false` · **Risk: Low** (owners unaffected; solo → no impact).

**8.2 — Optional member-privilege hardening** — ✅ **already set** (audit:
`members_can_fork_private_repositories = false`). Command for reference:
`gh api -X PATCH orgs/chinoba-lab -F members_can_fork_private_repositories=false`.
Verify via the same field. **Risk: Low.**

---

## 9 · Teams (governance scaffold — org-level, not repo files)

> ✅ **Already created** (audit 2026-07-01): all eight teams exist —
> `owners`, `maintainers`, `core`, `interaction`, `execution`, `learning`,
> `design`, `docs`. No action needed. (Membership population is a later,
> separate step.)

> **Phase B — not Phase A.** Create teams **after repository security is
> established** (§5.2, §6, §7, §3). Teams exist to grant write and drive
> CODEOWNERS review; there is no benefit to creating them before the repos and
> the protections they govern are in place.

Included because teams are an org setting the guides require (Phase 0 §5) and
CODEOWNERS defaults depend on them.

- **API (per team):** `gh api -X POST orgs/chinoba-lab/teams -f name=core -f privacy=closed`
  — repeat for: `owners maintainers core interaction execution learning design docs`.
  ```bash
  for t in owners maintainers core interaction execution learning design docs; do
    gh api -X POST orgs/chinoba-lab/teams -f name="$t" -f privacy=closed
  done
  ```
- **Verify:** `gh api orgs/chinoba-lab/teams --jq '.[].slug'`
- **Expected:** the eight slugs · **Risk: Low** (empty teams; founder joins each).

---

## 10 · Phased execution order (risk-gated)

Apply low-risk, high-value settings first; establish repository security next;
create the governance structure once there are protected repos to govern; do the
access-removing item last.

### Phase A — Low-risk organization defaults — ✅ SATISFIED (audit 2026-07-01)

All five target states are already in place; **no action required**:
repository creation restricted to owners (§8.1), base permission = `read` (§2.1),
default branch = `main` (§2.2), private-repo forking disabled (§8.2), and the
baseline audit itself (0.3). Left here for the record and rollback reference.

### Phase R — Establish repository security (the real next step)

Turn on detection so there is something for teams to govern.

- **§5.2** new-repository security defaults — ✅ **done (2026-07-01)**.
- **§7.1** Actions allowed-actions policy (`all` → `selected`) — ✅ **done (2026-07-01)**.

**Actionable items (next):**

1. **§4** (optional) per-repo classic branch protection for `.github` — Medium. ⬜ pending.
2. **§6.1–6.4** enable dependency graph / Dependabot / secret scanning on existing repos — Low.
3. **§7.3** fork-PR approval — Low. · **§7.2** read-only workflow token — ✅ already done.
4. **§6.5** secret-scanning **push protection** — Medium; enable knowingly.
5. **§3** org ruleset — ⛔ **blocked on Free plan** (HTTP 403); defer to a GitHub
   Team upgrade, or use §4 per-repo protection instead.

### Phase B — Organization structure — ✅ SATISFIED (audit 2026-07-01)

The eight teams (§9) already exist. Only membership population remains, as a
later step.

### Phase C — Access-removing — ✅ SATISFIED (audit 2026-07-01)

**§1.1** require 2FA is **already enabled** org-wide. No action.

### Deferred / conditional (not scheduled in A–C)

- **§2.3 Discussions** — **Optional**; enable only when a discussion surface is wanted.
- **§5.1 Private Vulnerability Reporting** — enable **when the org contains
  production code repositories** to report against.
- **§6.6 code scanning** — deferred to **Phase 3** (needs code repos).
- **§4 classic branch protection** — **skip** unless a per-repo need arises.

---

## 11 · One-shot verification (after applying)

```bash
gh api orgs/chinoba-lab --jq '{
  twofa:.two_factor_requirement_enabled,
  member_create:.members_can_create_repositories,
  base:.default_repository_permission
}'   # expect {"twofa":true,"member_create":false,"base":"read"}

gh api orgs/chinoba-lab/teams --jq '.[].slug'                 # eight teams (already exist)
gh api orgs/chinoba-lab/rulesets --jq '.[].name'             # ⛔ 403 on Free plan (see §3)
gh api orgs/chinoba-lab/actions/permissions/workflow          # read-only default token
gh api repos/chinoba-lab/.github --jq '.security_and_analysis' # features enabled
gh repo list chinoba-lab --json name --jq '.[].name'          # only: .github
```

---

## 12 · Coverage map — this plan vs. the existing guides

| Category | Source | New here? |
|---|---|---|
| Organization Security (2FA, owners) | Phase 0 §1, §4 | — |
| Repository Defaults (base perm, branch, discussions) | Phase 0 §3 · Phase 1 §2 | — |
| Rulesets | Phase 1 §8 | — |
| Branch Protection (classic) | governance docs | clarified as optional |
| Security Features (PVR, new-repo defaults) | Phase 0 §6/§7 · Phase 1 §2 | partially |
| Dependabot / Code / Secret scanning | — | **recommended addition** |
| Actions Permissions | — | **recommended addition** |
| Repository Creation Policies | Phase 0 §2 | — |

---

← Back to [`docs/implementation/`](README.md) · [`IMPLEMENTATION_PLAN.md`](IMPLEMENTATION_PLAN.md) · [`PHASE_0_SECURITY_CHECKLIST.md`](PHASE_0_SECURITY_CHECKLIST.md) · [`PHASE_1_IMPLEMENTATION_GUIDE.md`](PHASE_1_IMPLEMENTATION_GUIDE.md)
