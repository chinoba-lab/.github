# Organization Foundation — Completion Report

**The `chinoba-lab` organization foundation (security, governance, profile, and
default health files) is complete. This report is the phase's closing record and
the baseline that subsequent application/repository work can build on.**

*Verified against the live organization on 2026-07-01 (read-only `gh api`).
No organization setting was changed while producing this report.*

→ Detailed execution log: [`ORG_SECURITY_GOVERNANCE_PLAN.md`](ORG_SECURITY_GOVERNANCE_PLAN.md) ·
Roadmap: [`IMPLEMENTATION_PLAN.md`](IMPLEMENTATION_PLAN.md)

---

## 1 · What was accomplished

| Area | Outcome |
|---|---|
| **Phase 0 — Security & governance** | 2FA required; repo creation owner-only; base permission Read; eight teams created; SECURITY guidance drafted & published. |
| **Phase 1 — Org profile & health (repository side)** | `chinoba-lab/.github` public repo with `profile/README.md`, `CODE_OF_CONDUCT`, `CONTRIBUTING`, `SECURITY`, `SUPPORT`, `RELEASING`, `FUNDING`, `labels.yml`, `repo-template/`, issue/PR templates. |
| **Org Security & Governance execution** | Baseline audit; new-repository security defaults enabled (§5.2); Actions allowed-actions restricted to GitHub + verified creators (§7.1). |
| **Documentation** | Phased execution plan authored, kept in sync with each applied change, and audited against live state. |

Two organization settings were applied under review during this phase (both
approved, executed, verified, and documented):

1. **§5.2 — new-repository security defaults** (2026-07-01)
2. **§7.1 — Actions allowed-actions policy** (2026-07-01)

All other target settings were found **already in place** by the baseline audit.

---

## 2 · Organization settings now in place (verified 2026-07-01)

| Setting | Value | Source |
|---|---|---|
| Two-factor authentication required | `true` | pre-existing |
| Members can create repositories | `false` (public & private) | pre-existing |
| Base member permission | `read` | pre-existing |
| Default branch for new repos | `main` | pre-existing |
| Members can fork private repos | `false` | pre-existing |
| Teams | `owners, maintainers, core, interaction, execution, learning, design, docs` (8) | pre-existing |
| Workflow default token | `read`, cannot approve PRs | pre-existing |
| Dependency graph — new repos | `true` | **applied §5.2** |
| Dependabot alerts — new repos | `true` | **applied §5.2** |
| Dependabot security updates — new repos | `true` | **applied §5.2** |
| Secret scanning — new repos | `true` | **applied §5.2** |
| Secret-scanning push protection — new repos | `true` | **applied §5.2** |
| Actions `allowed_actions` | `selected` (github-owned + verified; `patterns_allowed=[]`) | **applied §7.1** |
| Repositories | `.github` (public) only | pre-existing |
| Plan tier | `free` | — |

**Documentation ↔ reality:** the live audit above matches every ✅ status recorded
in `ORG_SECURITY_GOVERNANCE_PLAN.md`. No drift.

---

## 3 · Remaining optional items (not required for foundation)

These are safe to do later, per-repo or on demand — none blocks application work:

- **§4 — per-repo classic branch protection for `.github`.** Optional stand-in for
  the org ruleset (which is plan-blocked, §5). Add when the repo takes external PRs.
- **§6.1–6.4 — enable scanning on existing repos.** Only `.github` (docs) exists;
  new repos already inherit these via §5.2. Apply per repo as code repos arrive.
- **§6.5 — secret-scanning push protection on existing repos.** Medium risk (blocks
  pushes containing secrets); enable knowingly when it matters.
- **§7.3 — fork-PR workflow approval.** UI-only; relevant once public repos take PRs.
- **§2.3 — organization Discussions.** Optional surface; enable when wanted.

---

## 4 · Items intentionally deferred (with reasons)

| Item | Deferred until | Why |
|---|---|---|
| **§5.1 — Private Vulnerability Reporting** | production code repos exist | The `.github` docs repo has nothing to report against. |
| **§6.6 — code scanning (CodeQL)** | Phase 3 (code repos) | Applies to code, not docs; free only on public repos. |
| **§3 — org-wide rulesets** | GitHub Team upgrade | Blocked on Free — see §5 below. |

---

## 5 · Known GitHub Free-plan limitations

Confirmed against the live org (2026-07-01):

- **Org-wide rulesets are unavailable.** `GET /orgs/chinoba-lab/rulesets` returns
  **HTTP 403 — "Upgrade to GitHub Team to enable this feature."** Org-level branch
  rulesets cannot be created on Free. *Workaround:* per-repo classic branch
  protection (§4).
- **Secret scanning & code scanning are public-repo-only.** Fine today (our repos
  are public); private repos would need paid GitHub Advanced Security.
- **No `Internal` repository visibility.** Use `private`; not a foundation concern.

Dependency graph, Dependabot, Actions policy, teams, 2FA, and base-permission
controls are **all fully available on Free** and are in place.

---

## 6 · Recommendations for future upgrades (GitHub Team)

Upgrade to **GitHub Team** only when a concrete need appears — not preemptively:

1. **Org-wide rulesets** — the strongest reason: one ruleset auto-protects the
   default branch of every repo (current & future). Replaces per-repo §4 upkeep.
2. **Advanced security on private repos** — if any graduated repo must be private
   and still needs secret/code scanning.
3. **Internal visibility** — if a shared library should be org-visible but not public.

Until then, the Free-plan posture is sound: per-repo protection covers the
ruleset gap, and all detection features work on the public repos we have.

---

## 7 · Verification summary

- ✅ **Documentation matches live organization state** — §2 audit reconciles with
  every status in `ORG_SECURITY_GOVERNANCE_PLAN.md`.
- ✅ **No outstanding *required* tasks** — all foundation items are done or
  deliberately deferred; every remaining item is optional or plan-gated.
- ✅ **Repository is internally consistent** — roadmap, execution plan, and this
  report agree on scope, status, and the Free-plan blocker.

---

## 8 · Assumptions future development can safely make

Application and repository work may proceed assuming:

1. **The org is hardened.** 2FA is enforced, only owners create repos, members
   default to Read, and the workflow token is read-only.
2. **New repositories start secure by default.** Any repo created now inherits
   dependency graph, Dependabot alerts/updates, secret scanning, and push
   protection automatically (§5.2) — no per-repo setup needed for these.
3. **Actions are least-privilege org-wide.** Only GitHub-owned and verified-creator
   actions run; add a repo/workflow's third-party actions to `patterns_allowed`
   as a deliberate step (§7.1).
4. **Teams exist and map to layers** (`core, interaction, execution, learning,
   design, docs`, plus `owners`/`maintainers`) — CODEOWNERS can reference them;
   membership is populated as people join.
5. **Branch protection is per-repo for now.** Until a GitHub Team upgrade, protect
   each new repo's default branch individually (§4); do not assume an org ruleset.
6. **`.github` publishes org-wide defaults** — profile, community-health files, and
   issue/PR templates are inherited by repos that lack their own.

> **Next focus:** application and repository development (bringing code repos to
> standard and graduating them per `IMPLEMENTATION_PLAN.md` Phases 2–7) — **not**
> further organization setup. The foundation is complete.

---

← Back to [`docs/implementation/`](README.md) · [`IMPLEMENTATION_PLAN.md`](IMPLEMENTATION_PLAN.md) · [`ORG_SECURITY_GOVERNANCE_PLAN.md`](ORG_SECURITY_GOVERNANCE_PLAN.md)
