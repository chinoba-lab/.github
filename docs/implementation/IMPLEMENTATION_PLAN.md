# IMPLEMENTATION_PLAN.md

**The master checklist for building the Chinoba GitHub Organization (`chinoba-lab`).**

*Companion to the architecture in [`README.md`](../../README.md) and [`docs/`](../).
This is a planning artifact — it does not itself create or move any repository.*

---

> ## ✅ Organization Foundation phase — COMPLETE (2026-07-01)
>
> Phases 0–1 and the organization security & governance work are **done and
> verified against the live org**. Settings in place: 2FA required, repo creation
> owner-only, base permission Read, eight teams, read-only workflow token,
> new-repo security defaults on, Actions restricted to GitHub + verified creators;
> the `.github` profile and community-health files are published.
>
> **Subsequent work should focus on application and repository development**
> (Phases 2–7 below) — bringing code repos to standard and graduating them — **not
> further organization setup.** Only two items remain org-level: per-repo branch
> protection (§4, optional) and org-wide rulesets (blocked on GitHub Free).
>
> → Full closing record: [`FOUNDATION_COMPLETION_REPORT.md`](FOUNDATION_COMPLETION_REPORT.md) ·
> execution log: [`ORG_SECURITY_GOVERNANCE_PLAN.md`](ORG_SECURITY_GOVERNANCE_PLAN.md)

---

## 0 · Status snapshot (verified 2026-07-01)

What is actually true today, confirmed against GitHub and chinoba.org:

| Fact | State | Implication |
|---|---|---|
| `chinoba-lab` org | **Exists**, created 2026-06-30, **0 public repos**, free plan | Empty → no name collisions; safe to migrate into |
| Org metadata | name *Chinoba*, `hello@chinoba.org`, Japan, blog `chinoba.org` | Identity already set; profile README still missing |
| `chinoba-lab/.github` | **Does not exist** | Must be created (the one new repo we create) |
| Org 2FA requirement | **Disabled** | Security gap — enable before adding members |
| Org repo-creation policy | **Any member can create** | Weakens the graduation gate — restrict to owners |
| Personal account | **37 repos, none archived** | Legacy/archive work is all still pending |
| `chinoba-site` (website) | Exists, public, on personal | Now mapped as Official; must migrate + carry domain |
| Curated official repos | 10 exist on personal, all public | Ready to graduate by transfer |
| Pre-architecture repos | ~15 older repos exist | Need one-time disposition (archive/consolidate) |
| Internal repos | **Not available on free plan** | "Internal" = use **private**; or upgrade plan |

### Resolved decisions (2026-07-01)

| # | Decision | Resolution | Effect |
|---|---|---|---|
| 1 | chinoba.org hosting | **Externally hosted** (not GitHub Pages) | Website migration is **ownership-first, not a hosting migration**. The production deployment path stays **unchanged** for now — no DNS change, no host switch. See Phase 2. |
| 2 | Org security posture | **Harden in Phase 0** | Require 2FA, restrict repo creation to owners, define owner/member roles, publish SECURITY.md guidance, run a governance checklist. See [`PHASE_0_SECURITY_CHECKLIST.md`](PHASE_0_SECURITY_CHECKLIST.md). |

> **No repositories are created or transferred yet.** Phase 0 is settings and
> documentation only.

---

## 1 · Review findings (what validation surfaced)

### 1.1 Architecture consistency — PASS, with two fixes applied
- **Repository names** are consistent across every document **and match GitHub
  reality** (all 10 official + 7 legacy names verified live). No drift.
- **Two fixes made** during this review (genuine design problems):
  1. `chinoba-site` (the website) was absent from the architecture though the
     roadmap names a Website phase → added to the repo map & strategy as Official.
  2. The repo map claimed to cover "every Chinoba repository" while omitting ~20
     repos → softened, plus a *Pre-architecture* section added for honesty.
- **No contradictory duplication.** Recurring concepts (Bench/Commons,
  graduation, "transfer not copy") repeat by design across self-contained,
  back-linked docs; the graduation gate and repo checklist are cross-referenced,
  not duplicated.

### 1.2 Ecosystem comparison — gaps identified
chinoba.org presents six surfaces (Research · Knowledge Artifacts · Experiments
· Channels · Activity · Seminars). The GitHub architecture covers **Experiments
/ Open Source** well. Gaps relative to the full ecosystem:

| Surface | Covered? | Gap / action |
|---|---|---|
| Open Source (code) | ✅ | Fully mapped |
| Website | ⚠️ now mapped | Migrate `chinoba-site` (Phase 2) |
| Org profile | ❌ | Create `.github` profile (Phase 1) |
| Books (22+) | ➖ | Lives on Kindle/`chinoba.org/library` — link, don't host |
| YouTube / Blog / LinkedIn | ➖ | External channels — link from profile only |
| Research (15 areas) | ⚠️ partial | Org docs cover 6 themes; 15 site areas are broader — keep research canonical on chinoba.org, link from org |
| Seminars / Activity | ➖ | Out of scope for the org; site concern |

> Recommendation: the org is the home of **code + the ecosystem's operating
> docs**. Books, research narrative, seminars stay canonical on chinoba.org; the
> org **links** to them (already done in the profile draft). Do not duplicate.

### 1.3 Security & org-settings findings (act in Phase 0)
- Enable **2FA requirement** for the org.
- Set **default member repo permission** appropriately and restrict **repo
  creation to owners** so graduation stays a deliberate decision.
- Decide branch-protection baseline (already specified in governance docs).

> These findings are now operationalized as a standalone, runnable checklist:
> [`PHASE_0_SECURITY_CHECKLIST.md`](PHASE_0_SECURITY_CHECKLIST.md). It is
> preparation only — settings are applied by the owner when ready; no repo is
> created or transferred.

---

## 2 · Repository strategy decisions (merge / split / keep / future)

### Keep & graduate (Official → org) — 11 repos
The 10 curated official repos + `chinoba-site`. No changes to their boundaries.

### Split — none recommended
The `-core` / `-v2` / `-k2` granularity is already right-sized. No repo shows
evidence of bundling unrelated concerns. **Do not split for its own sake.**

### Merge — the design-note cluster (9 repos → 1, or fold into the site)
`time-aware-data-for-ai`, `multi-agent-orchestration-design`,
`llm-agent-design-notes`, `user-behavior-event-design`, `decision-metric-design`,
`social-context-inference`, `ai-decision-visualization`,
`multi-agent-local-value-system`, `decision-pipeline-reference`.

> **Reasoning:** nine separate repos, each a short set of *design principles*
> (not software), fragment the namespace and dilute the official signal.
> Consolidate into a single `design-notes` repo **or** migrate the content into
> `chinoba.org/research`. They are notes about the architecture, not parts of it.

### Archive in place (Bench, frozen) — legacy + superseded prototypes
- **Curated legacy (7):** the `-v2`/`-core`/`-k2` predecessors (per architecture).
- **Pre-architecture prototypes (6):** `decision-trace-engine`,
  `decision-trace-platform`, `decision-trace-viewer`, `judgment-structure-core`,
  `Decision-Oriented-Signal-Platform`, `ai-decision-system-map`.

> **Reasoning:** all superseded by the current layered architecture. Archiving
> (not deleting, not moving) preserves every published URL while making the
> Commons read as a single clean architecture.

### Keep personal / private (do not publish to org) — adjacent work
- `chat-ai-platform` (**private** product), `Synapse-Insights` (analytics
  product), `runtime-chat-experimental` (experiment), `masao-watanabe-ai`
  (profile). These are not Chinoba OSS core. "Internal" isn't available on the
  free org plan, so these **stay private/personal** until/unless they graduate.

### Future repositories (plan, do **not** create now)
| Future repo | Purpose | Trigger to create |
|---|---|---|
| `chinoba-lab/.github` | Org profile + community health | **Phase 1 (create now)** |
| `ecosystem` (or `chinoba`) | Public home for this architecture + roadmap | When docs should be public |
| `design-notes` | Consolidated design principles | If merging cluster in-repo (vs site) |
| Coordination-stream code | Governance / multi-agent-coordination / runtime-society currently are themes only | When code, not just research, exists |
| Language SDKs / clients | Per-language client libs | On real downstream demand |

> Only `.github` is created in this plan. Everything else is deferred until its
> trigger fires — honoring "no unnecessary repository is created."

---

## 3 · Migration safety review (Bench → Commons)

| Safety property | Verdict | Note |
|---|---|---|
| Existing URLs remain valid | ✅ | GitHub transfer installs automatic redirects |
| Redirects preserved | ✅ *with caveats* | Never recreate a transferred name on personal; don't rename post-move; package/import paths don't auto-redirect |
| Transfer order correct | ✅ | Spec → kernel → ledger → view → interaction → execution → learning → studio → starters; website independent/early |
| No unnecessary repo created | ✅ | Only `.github` is created |
| Name collisions in org | ✅ none | Org is empty |
| Domain continuity (chinoba.org) | ✅ unaffected | External host; ownership-only migration, deployment & DNS unchanged (Phase 2) |

→ Full procedure and caveats: [`docs/organization/migration-plan.md`](../organization/migration-plan.md)

---

## 4 · Implementation roadmap (phased)

Each phase: **Objectives · Deliverables · Dependencies · Completion criteria.**
Phases 0–1 are prerequisites; 2–5 can overlap once 0–1 are done.

### Phase 0 — Organization foundation & security  · difficulty: Low
- **Objectives:** make the org safe and policy-correct before anything lands.
- **Deliverables:**
  - **Require 2FA** for all members.
  - **Restrict repository creation to owners** (members cannot create repos).
  - **Base member permission** set (Read) so members see code but don't write.
  - **Owner / member roles defined** (org-level GitHub roles, mapped to the
    project roles in [`docs/governance/teams-and-roles.md`](../governance/teams-and-roles.md)).
  - **SECURITY.md guidance** drafted (published with `.github` in Phase 1).
  - **Teams** created (`owners`, `maintainers`, `core`, `interaction`,
    `execution`, `learning`, `design`, `docs`).
  - **Branch-protection baseline** documented.
  - **Governance checklist** run.
- **Driven by:** [`PHASE_0_SECURITY_CHECKLIST.md`](PHASE_0_SECURITY_CHECKLIST.md).
- **Dependencies:** none (org already exists).
- **Completion:** `gh api orgs/chinoba-lab` shows 2FA required, member repo
  creation disabled, base permission set; teams exist; SECURITY.md content ready.

### Phase 1 — Organization profile  · difficulty: Low
- **Objectives:** give the org a public face.
- **Deliverables:** create `chinoba-lab/.github`; publish `profile/README.md`
  from [`profile/README.md`](../../profile/README.md); add org-level
  `CODE_OF_CONDUCT`, `SECURITY`, `CONTRIBUTING`, `SUPPORT`, issue/PR templates.
- **Dependencies:** Phase 0 (teams for CODEOWNERS defaults).
- **Completion:** github.com/chinoba-lab renders the profile; health files resolve.

### Phase 2 — Website (ownership only)  · difficulty: Low–Medium
- **Decision applied:** chinoba.org is **externally hosted**. This phase is a
  **code/ownership migration only** — **not** a hosting migration. The
  production deployment path and DNS stay **unchanged**.
- **Objectives:** move ownership of `chinoba-site` to the org while production
  keeps deploying exactly as it does today.
- **Deliverables:** transfer `chinoba-site` → org; confirm the old URL redirects;
  reconnect the external host's Git integration to the org-owned repo **only if**
  the transfer interrupts auto-deploys (a connection re-point, not a host/DNS
  change). **No domain, DNS, or hosting-platform change is made.**
- **Dependencies:** Phase 1. Independent of the core dependency chain — but may
  be **deferred** to a low-traffic window since it touches the live front door.
- **Completion:** `chinoba-lab/chinoba-site` owns the source; old repo URL
  redirects; chinoba.org still serves from the unchanged external deployment.

> **Out of scope (future, optional):** moving hosting onto org-managed infra or
> GitHub Pages. Not planned now; the deployment path remains as-is.

### Phase 3 — Research / runtime repositories  · difficulty: Medium–High
- **Objectives:** graduate the 10 official code repos.
- **Deliverables:** each repo brought to standard *on the Bench first*
  ([`repository-template.md`](../standards/repository-template.md)), then
  transferred in dependency order, assigned to its layer team, topic-tagged,
  branch-protected, redirect-verified.
- **Dependencies:** Phases 0–1; standards finalized.
- **Completion:** all 10 live under `chinoba-lab`, each passing the repo
  checklist; old URLs redirect; repository map updated.

### Phase 4 — Shared libraries & standardization  · difficulty: Medium
- **Objectives:** make the migrated core dependable as libraries.
- **Deliverables:** consistent LICENSE/CHANGELOG/versioning across core repos;
  CODEOWNERS everywhere; cross-repo links repointed to org; release tags.
- **Dependencies:** Phase 3.
- **Completion:** every official repo meets all standards; versioned releases exist.

### Phase 5 — Examples & starter kits  · difficulty: Low–Medium
- **Objectives:** give newcomers a supported on-ramp on the Commons.
- **Deliverables:** both starter kits graduated and verified end-to-end against
  the now-org-hosted core; demos linked from the profile/site.
- **Dependencies:** Phases 3–4 (starters depend on core).
- **Completion:** a newcomer can clone a starter kit from the org and run the
  signal → decision → trace flow.

### Phase 6 — Community  · difficulty: Medium (ongoing)
- **Objectives:** open the org to contributors safely.
- **Deliverables:** activate Discussions; finalize CONTRIBUTING flow; label
  scheme; governance/decision-rights in force; first "good first issue"s.
- **Dependencies:** Phases 1–5.
- **Completion:** an external contributor can go issue → PR → review → merge,
  governed by CODEOWNERS and the decision matrix.

### Phase 7 (cleanup / future) — Bench housekeeping  · difficulty: Low (volume)
- **Objectives:** leave the Bench tidy and honest.
- **Deliverables:** archive curated legacy (7) + superseded prototypes (6) with
  successor banners; resolve the design-note cluster (merge or fold into site);
  confirm adjacent products stay private.
- **Dependencies:** Phase 3 (successors live before banners point to them).
- **Completion:** Bench shows only profile + active experiments + archived
  reference; repository map reflects final state.

---

## 5 · Master checklist (prioritized, with difficulty & dependencies)

Priority: **P0** blocking · **P1** core · **P2** quality · **P3** future.
Difficulty: ●○○ Low · ●●○ Medium · ●●● High.

### P0 — Foundation & security (do first) → [`PHASE_0_SECURITY_CHECKLIST.md`](PHASE_0_SECURITY_CHECKLIST.md) — ✅ **COMPLETE (2026-07-01)**
- [x] Require 2FA for all members — ●○○ — *(verified `true`)*
- [x] Restrict repo creation to owners — ●○○ — *(verified `false`)*
- [x] Set base member permission (Read) — ●○○ — *(verified `read`)*
- [x] Define owner/member roles — ●○○ — *(governance docs ✓)*
- [x] Draft SECURITY.md guidance (publish in Phase 1) — ●○○ — *(published in `.github`)*
- [x] Create the eight teams — ●○○ — *(all 8 verified)*
- [x] Document branch-protection baseline — ●○○ — *(governance docs ✓; org ruleset blocked on Free)*
- [x] Run the org governance checklist — ●○○
- [x] **Owner decision:** chinoba.org hosting → **external, ownership-only migration** ✓
- [x] **Owner decision:** security → **harden in Phase 0** ✓

### P1 — Profile & website
- [x] Create `chinoba-lab/.github` — ●○○ — *(P0)* ✅
- [x] Publish org profile from `profile/README.md` — ●○○ — *(.github)* ✅
- [x] Add org community-health files — ●○○ — *(.github)* ✅
- [ ] Transfer `chinoba-site` ownership only; verify redirect; **no DNS/host change** — ●○○ — *(profile; Phase 2 — not started)*

### P1 — Core migration (dependency order)
- [ ] Bring each official repo to standard on the Bench — ●●○ — *(standards)*
- [ ] Transfer `decision-trace-model-v2` — ●●○
- [ ] Transfer `decision-runtime-core` — ●●○ — *(model)*
- [ ] Transfer `decision-trace-ledger-core-k2` — ●●○ — *(runtime)*
- [ ] Transfer `dtm-view-core-v2` — ●●○ — *(ledger)*
- [ ] Transfer `interaction-core-v2` — ●●○ — *(runtime)*
- [ ] Transfer `multi-agent-orchestrator-core-v2` — ●●○ — *(runtime)*
- [ ] Transfer `decision-trace-gnn-core-v2` — ●●○ — *(ledger)*
- [ ] Transfer `decision-trace-studio-v2` — ●●○
- [ ] Per repo: assign team, topics, branch protection, verify redirect — ●●○
- [ ] Transfer both `light-dtm-starter-kit-cs*` (last) — ●●○ — *(all core)*

### P2 — Standardization & community
- [ ] CODEOWNERS + CHANGELOG + LICENSE consistent across official repos — ●●○ — *(core migrated)*
- [ ] Repoint cross-repo links to org; tag releases — ●●○
- [ ] Verify starter kits run against org-hosted core — ●●○ — *(core)*
- [ ] Activate Discussions; finalize CONTRIBUTING; label scheme — ●●○
- [ ] First good-first-issues — ●○○

### P3 — Cleanup & future
- [ ] Archive curated legacy (7) with successor banners — ●○○ — *(successors live)*
- [ ] Archive superseded prototypes (6) — ●○○
- [ ] Resolve design-note cluster (merge → `design-notes` or fold into site) — ●●○
- [ ] Confirm `chat-ai-platform` / `Synapse-Insights` stay private — ●○○
- [ ] Re-evaluate future repos (`ecosystem`, SDKs, coordination code) against triggers — ●●○

---

## 6 · Migration order (one-line reference)

```
Phase 0 settings/teams
  → .github (profile + health)
    → chinoba-site (ownership only; deploy & DNS unchanged)   [independent]
      → model-v2 → runtime-core → ledger-k2 → view-v2
                 → interaction-v2 → orchestrator-v2 → gnn-v2 → studio-v2
        → starter-kit-cs-v2 / -cs    [last]
          → standardize → community
            → archive legacy + prototypes → resolve design-notes
```

---

## 7 · Local workspace review

The `Freedom_BIZ/github/chinoba-lab/` workspace structure is **suitable for
long-term maintenance** as-is:

- Hub-and-spoke `docs/` mirrors the existing repo's proven convention.
- Topic folders (`organization`, `architecture`, `governance`, `standards`) are
  stable categories that won't churn.
- `profile/` cleanly separates publishable content from internal docs.

**No structural change recommended.** Only one lightweight addition is worth it
*as the plan executes* (not now): a `CHANGELOG.md` or `docs/decisions/` for
recording structural decisions (graduations, archives) as they happen — the
governance docs call for a traceable record, and a dated log is the simplest
home for it. Defer until the first real graduation.

---

## 8 · Future expansion notes

- **Coordination Stream becomes code.** Today Governance / Multi-Agent
  Coordination / Runtime Society are research themes with no repos. When they
  acquire implementations, add them as official repos under new layer/stream
  topics — the architecture already anticipates this without restructuring.
- **`ecosystem` repo.** If this architecture should be public, publish it as
  `chinoba-lab/ecosystem` rather than editing the personal reference repo.
- **Plan upgrade.** Internal repos and advanced security need a paid org plan;
  upgrade only when a real need (internal-only shared lib, security scanning)
  appears.
- **SDKs / clients.** Add per-language clients only on demonstrated downstream
  demand; premature SDKs are maintenance debt.
- **Books & research stay canonical on chinoba.org.** The org links to them; it
  does not mirror them. Revisit only if a docs-as-code workflow is wanted.

---

← Back to the [chinoba-lab workspace](../../README.md) · [docs/implementation/](README.md)
