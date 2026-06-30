# Decision Rights

*Governance · Chinoba Ecosystem*

Who decides what — and how each decision leaves a trace. This document applies
Chinoba's central idea, **Signal → Decision**, to the act of running the
organization itself.

---

## The organization as a decision runtime

Chinoba's runtime turns signals into accountable decisions and records the trace
as part of the decision. `chinoba-lab` is governed the same way:

| In the runtime | In the organization |
|---|---|
| AI emits a **signal** | A contributor opens a proposal / PR / experiment |
| The runtime applies **decision logic** | Maintainers review against the standards |
| **Boundaries** are enforced | Branch protection, CODEOWNERS, scope checks |
| A **human gate** is invoked | An owner/maintainer approves or says no |
| The **decision trace** is recorded | The merge, release note, or changelog entry |

> A pull request is a signal. A merge is a decision. The git history is the
> ledger. Governance is just the runtime, run by people.

---

## Decision matrix

Who holds authority for each class of decision:

| Decision | Proposed by | Decided by | Recorded in |
|---|---|---|---|
| Code change to an official repo | Anyone | Owning team (CODEOWNERS) | PR + git history |
| New release / version | Maintainer | Owning team | Release + changelog |
| **Repo graduation** (Bench → Commons) | Author | Owners | Migration log + repo map |
| New official repository | Anyone | Owners | Org changelog |
| Archiving / deprecating a repo | Maintainer | Owners | Archive banner + repo map |
| New team / role | Maintainer | Owners | This document + org settings |
| Standards or policy change | Anyone | Owners | The relevant `docs/` file |
| Naming / new role-word | Anyone | Owners | [`naming-conventions.md`](../organization/naming-conventions.md) |

At launch the Owner and the Maintainer are the same person; the matrix still
holds, so authority never has to be re-derived as the project grows.

---

## The right to say no

The Governance theme insists on "the right to say no." In the org this is not
implicit — it is a named authority:

> Any Owner may **decline** any change, graduation, or release, for any reason
> consistent with the philosophy and standards. A decline is recorded with its
> reason, exactly as an accountable decision should be.

Saying no is not friction; it is the boundary that keeps the Commons trustworthy.

---

## Traceability of governance

Every decision above produces an artifact that can be replayed later:

- **Code/release** → PR thread, commit, release notes.
- **Structural** (graduation, new repo, archive) → an entry in the org
  changelog and an update to [`../architecture/repository-map.md`](../architecture/repository-map.md).
- **Policy** → a commit to the relevant `docs/` file in this workspace.

This mirrors the runtime's promise: the decision and its trace exist
**independently of the outcome**. Whether a graduated repo later thrives or is
deprecated, the record of *why it graduated* remains.

---

← Back to [Governance](README.md) · [chinoba-lab workspace](../../README.md)
