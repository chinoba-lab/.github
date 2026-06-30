# Personal vs Organization — The Boundary

*Organization · Chinoba Ecosystem*

The single most important decision in this architecture: **what lives on the
personal account, and what lives on the organization.** Everything else follows
from this boundary.

---

## The Bench and the Commons

| | **The Bench** | **The Commons** |
|---|---|---|
| **Surface** | `Masao-Watanabe-AI` (personal) | `chinoba-lab` (organization) |
| **Question it answers** | "What is Masao exploring?" | "What is official Chinoba?" |
| **Stage of trust** | Provisional | Earned |
| **Audience** | The author; curious onlookers | The public; contributors; dependents |
| **Stability promise** | None — may break or vanish | Supported — versioned, documented |
| **Lifecycle** | Born, iterated, graduated or retired | Maintained, governed, depended upon |

> The Bench is where intelligence is a **signal**. The Commons is where it
> becomes a **decision**. The boundary between them is a graduation gate — the
> ecosystem's own *Signal → Decision*.

---

## What lives on the Bench (personal)

The personal account remains the home of everything **pre-official**:

- **Personal profile** — the `masao-watanabe-ai/masao-watanabe-ai` profile repo.
- **Experiments** — research spikes, throwaway probes, "does this even work?"
- **Prototypes** — early implementations not yet ready to be depended on.
- **Sandbox repositories** — scratch space, demos-in-progress, forks for study.
- **The legacy archive** — superseded v1 repositories, kept for reference (see
  [`repository-strategy.md`](repository-strategy.md)).

The Bench is **deliberately permissive**. Things here are allowed to be messy,
unfinished, or wrong. That freedom is the point — it is where ideas are cheap.

---

## What lives on the Commons (organization)

The organization hosts only **official Chinoba**:

- **The core implementation repositories** — the current (`-v2` / `-core` /
  `-k2`) repos that make up the five-layer architecture.
- **Specifications** — protocols and models meant to be cited and built against.
- **Starter kits & supported examples** — entry points people are told to use.
- **Organization meta** — the `.github` profile, community-health files, and
  this kind of ecosystem documentation.

The Commons is **deliberately strict**. Everything here meets the
[standards](../standards/README.md), has an owner, and carries a stability
promise. That discipline is the point — it is what makes Chinoba dependable.

---

## The decision rule

When you cannot tell where something belongs, ask one question:

> **Should a stranger be able to depend on this?**

- **No / not yet** → the Bench.
- **Yes** → the Commons.

Dependence is the dividing line because Chinoba's whole thesis is relational:
the moment others build a relationship of reliance on a piece of work, that
work has taken on an obligation — and obligations belong to the named,
accountable entity, not the personal sandbox.

---

## What never moves

Two things stay on the Bench permanently, by design:

1. **The personal profile repo** — it describes a person, not the project.
2. **The legacy archive** — superseded repos are frozen where they are, so that
   every URL already published in books, blogs, and the existing documentation
   keeps resolving. Moving them would serve tidiness at the cost of history.

→ How the move actually happens: [`migration-plan.md`](migration-plan.md)

---

← Back to [Organization](README.md) · [chinoba-lab workspace](../../README.md)
