# Organization Architecture

*How the `chinoba-lab` organization maps onto Chinoba's research.*

The existing documentation defines Chinoba's intellectual architecture: one
**philosophy**, two **research streams**, and five **implementation layers**.
This section does not redefine any of that. It shows how the **organization
structure** is a faithful projection of it — so that the shape of the GitHub org
*is* the shape of the research.

→ The source architecture (reference): [`../../../docs/architecture/`](../../../docs/architecture/)

---

## The projection

Three of Chinoba's concepts each become a concrete organizational mechanism:

| Chinoba concept (existing) | Organizational mechanism (this workspace) |
|---|---|
| **Philosophy** — Intelligence as Relationship | The two-surface model: Bench ↔ Commons, related by graduation |
| **Five implementation layers** | Repository topics + governance teams, one per layer |
| **Two research streams** | `stream-decision` / `stream-coordination` topics on every repo |
| **The runtime is the only decision authority** | The org is the only authority of record for "official Chinoba" |

> The architecture is not described *to* the org as an afterthought. The org is
> **built in its image**.

---

## The five layers, as the org holds them

The existing architecture organizes the implementation into five layers. On the
org, each layer is a **team** (who maintains it) and a **topic** (how you find
it). No new layers are introduced — the org inherits these exactly.

```
Interaction → Signal → Decision (Runtime) → Boundary → Human → ┬ Execution
                                                               └ Log (decision trace)
```

| Layer | Stream served | Official repos on the org |
|---|---|---|
| **Interaction** | Both | `interaction-core-v2` |
| **Core / Decision Runtime** | Decision | `decision-runtime-core`, `decision-trace-model-v2`, `decision-trace-ledger-core-k2`, `dtm-view-core-v2` |
| **Execution** | Decision | `multi-agent-orchestrator-core-v2` |
| **Learning** | Decision | `decision-trace-gnn-core-v2` |
| **Design** | Both | `decision-trace-studio-v2` |

→ The complete repo-by-repo map: [`repository-map.md`](repository-map.md)

---

## The two streams, as topics

The existing research runs as two streams. Rather than split the org in two,
every repo is **tagged** with the stream it primarily serves — so the streams
remain visible without fragmenting the namespace:

- **`stream-decision`** — producing accountable decisions (most core repos).
- **`stream-coordination`** — coordinating humans and AI around them.

As the existing docs note, the Coordination Stream is not a single layer of
code; it **emerges from the behavior** of systems built on the decision layers.
The org reflects that honestly: it ships the decision machinery, and tags the
coordination dimension wherever it appears, rather than pretending there is a
"coordination repo".

---

## Why the org mirrors the runtime

The runtime is Chinoba's *single point of decision* — every other layer produces
signals, follows decisions, or makes them understandable, but none of them
decide. The organization adopts the same shape one level up:

> `chinoba-lab` is the single point of authority for what is **official**
> Chinoba. The personal Bench produces candidates (signals); graduation is the
> decision; the Commons is the accountable record.

This is the deepest inheritance in the whole workspace: the way Chinoba reasons
about decisions is the way Chinoba is *organized*.

---

← Back to the [chinoba-lab workspace](../../README.md)
