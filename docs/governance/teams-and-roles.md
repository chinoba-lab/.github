# Teams and Roles

*Governance · Chinoba Ecosystem*

The people-structure of `chinoba-lab`: the teams that hold repositories, the
roles people occupy, and the GitHub permissions each carries. Designed to be
correct for a one-maintainer start and to **scale without restructuring**.

---

## Teams

Teams map one-to-one onto the architecture's layers, plus a small set of
cross-cutting teams. A repo belongs to its layer team; that team owns it.

| Team | Owns | Maps to |
|---|---|---|
| `owners` | The org itself | Org administration & final authority |
| `maintainers` | All official repos | Cross-cutting maintenance |
| `core` | `decision-runtime-core`, `-model-v2`, `-ledger-core-k2`, `dtm-view-core-v2` | Core / Decision Runtime layer |
| `interaction` | `interaction-core-v2` | Interaction layer |
| `execution` | `multi-agent-orchestrator-core-v2` | Execution layer |
| `learning` | `decision-trace-gnn-core-v2` | Learning layer |
| `design` | `decision-trace-studio-v2` | Design layer |
| `docs` | `.github`, starter kits, ecosystem docs | Documentation & onboarding |

> At launch, one person (the founder) is the sole member of every team. The
> teams still exist from day one — so that adding the second maintainer is a
> single membership change, not a reorganization.

---

## Roles and permissions

Roles describe *what a person may do*; GitHub permission levels enforce it.

| Role | GitHub level | May… |
|---|---|---|
| **Owner** | Org owner | Administer the org, create/transfer/archive repos, set policy |
| **Maintainer** | Maintain / Admin on repo | Merge, release, manage issues, steward a repo |
| **Committer** | Write | Push branches, open PRs that may be merged after review |
| **Contributor** | Read + fork | Open issues and pull requests |
| **Community** | Read | Use, file issues, discuss |

The progression from Community → Contributor → Committer → Maintainer is a trust
relationship that is *earned and traceable* — the same shape as the research's
[Trust Infrastructure](../../../docs/themes/trust-infrastructure.md) theme:
trust is established, traced, and verified, not assumed.

---

## CODEOWNERS — accountability per repository

Every official repo carries a `CODEOWNERS` file naming the team responsible for
it. This makes ownership **mechanical**: GitHub requests the owning team's review
automatically on every PR.

```
# CODEOWNERS — example for a core repo
*           @chinoba-lab/core
/docs/      @chinoba-lab/docs
```

> No official repo is merged into without a named owner reviewing. "Who is
> responsible for what" stops being a question and becomes a file.

---

## Branch protection (the boundary)

Official repos enforce, at minimum:

- Default branch protected; no direct pushes.
- At least one approving review from the owning team.
- Required status checks pass before merge.
- The Owner retains the right to **say no** (block/close) on any change.

These are the "boundaries" of the Governance theme, made literal as repository
settings.

---

← Back to [Governance](README.md) · [chinoba-lab workspace](../../README.md)
