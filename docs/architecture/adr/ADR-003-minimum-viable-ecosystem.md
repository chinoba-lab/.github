# ADR-003 — Minimum Viable Ecosystem (MVE)

- **Status:** Proposed
- **Date:** 2026-07-01
- **Deciders:** Chinoba owner
- **Relates to:** [`../ECOSYSTEM_ARCHITECTURE_PROPOSAL.md`](../ECOSYSTEM_ARCHITECTURE_PROPOSAL.md) §7, §6, §9.5

---

## Context

Graduating all ten code repos at once is slow and risky. We want a **smallest
coherent slice** that proves Chinoba's core thesis — *AI is decision; decisions
are traced* — end to end, so a newcomer can run **signal → decision → trace**
with the fewest moving parts. That slice becomes the **first delivery target**
and orders the graduation work (proposal §6).

The dependency graph (proposal §5) is acyclic with `decision-trace-model-v2` as
the universal foundation and the starter kits as the top-level integrators, which
makes a minimal end-to-end path well-defined.

---

## Decision

**The Minimum Viable Ecosystem is four repositories:**

| Repo | Role in the MVE |
|---|---|
| `decision-trace-model-v2` | the shared language (keystone) |
| `decision-runtime-core` | makes the decision |
| `decision-trace-ledger-core-k2` | records the trace |
| `light-dtm-starter-kit-cs` | standalone starter that ties them into a runnable local flow |

This set runs the **signal → decision → trace** loop **locally, with no external
services**, and is the first graduation target.

A **Complete First Release** (post-MVE) adds the parts that make the flow *real*
and *inspectable*: `dtm-view-core-v2` (see the trace), `interaction-core-v2` (real
signals), and `light-dtm-starter-kit-cs-v2` (the full runtime-connected flow).

Execution (`orchestrator`), Learning (`gnn`), and Design (`studio`) are **outside
the MVE** — high value, but not required to demonstrate the core loop.
`chinoba-site` ships independently (front door; no code dependency).

---

## Options considered

1. **4-repo MVE: model + runtime + ledger + standalone starter** — *Chosen.*
   Smallest set that still shows the *whole* loop (decision **and** trace), self-contained.
2. **Starter kit only** — *Rejected.* A starter with no separately consumable
   core doesn't demonstrate the reusable architecture; hides the keystone.
3. **7-repo "Complete First Release" as the MVE** — *Rejected as too large.*
   Adds view/interaction/second-starter value but delays first delivery and
   couples more repos than needed to prove the thesis.
4. **3-repo (drop the ledger)** — *Rejected.* Without the ledger there is no
   *trace*; that would demonstrate "decision" but not the accountability that is
   the point of Chinoba.

---

## Consequences

**Positive**
- A concrete, small **first delivery target**; graduation prioritizes these four.
- Proves the core value proposition early, with minimal surface to support.
- Matches the front-of-graph order in proposal §6 (model → runtime → ledger → …).

**Negative / costs**
- Users see the *loop* but not yet the *lens* (`view`) — the trace is recorded but
  inspected only programmatically until the Complete First Release.
- Execution/Learning/Design value is deferred; early adopters can't yet act on,
  learn from, or design decisions through official tooling.

**Neutral**
- Defines "done" for the first milestone without constraining later phases.

---

## Open questions

1. Does the standalone starter record traces to the **real** `decision-trace-ledger-core-k2`
   (embedded/local mode), or to a **stub** local store? (Affects whether the ledger
   is truly in the MVE or only its interface.)
2. Is a trace meaningful to a newcomer **without** the `view`? If not, should
   `dtm-view-core-v2` be pulled **into** the MVE (making it 5 repos)?
3. Which **language** does the MVE ship in first, given ADR-001's polyglot stance
   and the `-cs` (C#/.NET) starter? Does MVE == the .NET path initially?
4. What is the **acceptance test** for "MVE complete" — a documented end-to-end
   scenario a stranger can run from a clean machine?

---

← [ADR index](.) · [Ecosystem Architecture Proposal](../ECOSYSTEM_ARCHITECTURE_PROPOSAL.md)
