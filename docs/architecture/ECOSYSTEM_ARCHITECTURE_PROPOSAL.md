# Chinoba Open-Source Ecosystem — Architecture Proposal

**A complete design for the Chinoba code ecosystem: its reusable core, its
repositories, their contracts and dependencies, the build order, and the
Minimum Viable Ecosystem — for review before any repository is created.**

*Phase 2 (Repository & Runtime Architecture) design artifact. It creates
nothing; it proposes. Grounded in the existing architecture docs
([`repository-map.md`](repository-map.md), [`README.md`](README.md),
[`../organization/repository-strategy.md`](../organization/repository-strategy.md),
[`../organization/naming-conventions.md`](../organization/naming-conventions.md)).*

---

## 0 · Scope, method, and one honesty caveat

- **Scope:** the ten official **code** repositories + the website, framed as an
  engineered ecosystem — plus a clear line around what is *research theme, not
  code* and what is *future, on trigger*.
- **Method:** the layers, streams, names, and target repos are **not invented
  here** — they come from the existing docs. This proposal adds the engineering
  view: reusable core, contracts, dependency graph, build order, MVE.
- **⚠ Caveat on "public API":** the source code of these repos is on the founder's
  personal account, **not in this workspace.** Every API surface below is a
  **proposed contract** inferred from the architecture and the naming grammar
  (`<domain>-<role>-<generation>`). Each must be **validated against the actual
  repo during graduation**, and corrected where reality differs. Treat them as
  design intent, not observed fact.

---

## 1 · Research themes reviewed

| Theme | Form today | In this proposal |
|---|---|---|
| **Philosophy — Intelligence as Relationship** | Framing | The ecosystem ships the *infrastructure of the relationship*, not models. |
| **Decision Stream** — accountable decisions | **Code** (the five layers) | The whole code ecosystem below. |
| **Coordination Stream** — coordinating humans + AI | **Research + a topic** | *Emerges from behavior*; **not** its own repos (see §8). Tagged `stream-coordination` where it appears. |
| **Five layers** — Interaction · Core/Runtime · Execution · Learning · Design | **Code** | The repository architecture (§4–§5). |
| **Coordination sub-themes** — Governance, Multi-Agent Coordination, Runtime Society | **Research only** | Deferred until they become code (§8). |
| **Trust Infrastructure** — access earned, traced, verified | Cross-cutting | Realized by the *ledger* (trace) + *runtime* (boundaries) + org governance. |

**Key reading:** only the **Decision Stream / five layers** is code today. The
Coordination Stream is deliberately *not* a repository — the existing
architecture is explicit that coordination emerges from systems built on the
decision layers. This proposal honors that: **no "coordination-core" repo.**

---

## 2 · Core reusable components (the load-bearing set)

Four repositories carry the weight of the whole ecosystem — everything else
composes them. These are the "depend-on-me" core:

| # | Component | Why it is core |
|---|---|---|
| **C1** | `decision-trace-model-v2` | **The keystone.** The shared language — what a Signal, Decision, Boundary, and Trace *are*. Every other repo speaks it. If one thing must be stable first, it is this. |
| **C2** | `decision-runtime-core` | **The kernel.** The single decision authority. Execution, Studio, and the starters all invoke it. |
| **C3** | `decision-trace-ledger-core-k2` | **The record.** Tamper-evident trace storage. Learning reads it; the runtime-connected flow writes to it. |
| **C4** | `dtm-view-core-v2` | **The lens.** Reads traces into something a human can inspect. Reused by Studio and the full starter. |

> **Recommendation — do not mint a new "protocol"/"SDK" package.**
> `decision-trace-model-v2` already *is* the protocol foundation. Adding a
> separate shared package would violate "no unnecessary repository" and split the
> source of truth. Language client SDKs are a *future, on-demand* concern (§8).

---

## 3 · Repository architecture (tiers)

The org namespace is flat; structure is expressed by **topics**, not folders
(per naming conventions). Logically, the repos stack in dependency tiers:

```
Tier 0  Foundation spec     decision-trace-model-v2
Tier 1  Core kernels        decision-runtime-core · decision-trace-ledger-core-k2 · dtm-view-core-v2
Tier 2  Layer services      interaction-core-v2 · multi-agent-orchestrator-core-v2 · decision-trace-gnn-core-v2
Tier 3  Design/simulate     decision-trace-studio-v2
Tier 4  Entry points        light-dtm-starter-kit-cs · light-dtm-starter-kit-cs-v2
Independent                 chinoba-site  (front door; no code dependency)
```

Every official repo carries: `chinoba` + `layer-<x>` + `stream-<y>` topics
(+ `starter-kit` where applicable).

---

## 4 · Per-repository specification

> Maturity vocabulary (from `RELEASING.md`): **stable · beta · spec-draft**.
> The **target** tier below is the maturity the repo should reach for first
> official release; **actual** current maturity requires a per-repo graduation
> audit (⚠ §0). Dependencies list *Chinoba* deps only (not third-party libs).

### Tier 0 — Foundation

**`decision-trace-model-v2`** · Core · `stream-decision` (serves both)
- **Purpose:** canonical protocol + reference types for the Decision Trace Model
  — the schema for Signals, Decisions, Boundaries, Verdicts, and Trace entries,
  and how a trace is structured, versioned, and validated.
- **Proposed public API:** type/schema definitions (`Signal`, `DecisionRequest`,
  `Decision`, `Boundary`, `Verdict`, `TraceEntry`); (de)serialization; schema
  validation; protocol-version negotiation. Likely: language types **+** a
  language-neutral JSON Schema.
- **Depends on:** — (none; foundation).
- **Target maturity:** **stable** (must be stable *before* dependents graduate).
- **Expected users:** every downstream repo; integrators; spec readers/auditors.

### Tier 1 — Core kernels

**`decision-runtime-core`** · Core · `stream-decision`
- **Purpose:** the Decision OS kernel — evaluates a signal within boundaries and
  produces a finalized `Decision` **and** its `Trace`. The only component that decides.
- **Proposed public API:** `decide(signal, context, boundaries) → {decision, trace}`;
  boundary/policy evaluation; human-in-the-loop hooks; pluggable evaluators.
- **Depends on:** `decision-trace-model-v2`.
- **Target maturity:** **stable** (kernel).
- **Expected users:** app developers; Execution, Studio, and both starters.

**`decision-trace-ledger-core-k2`** · Core · `stream-decision`
- **Purpose:** append-only, hash-chained, tamper-evident store of decision traces.
- **Proposed public API:** `append(trace) → receipt`; `get(id)`; `query(filter)`;
  `verify(range) → integrity`; chain export/import.
- **Depends on:** `decision-trace-model-v2`.
- **Target maturity:** **beta → stable**.
- **Expected users:** the runtime (writes); Learning (reads); Studio; auditors/compliance.

**`dtm-view-core-v2`** · Core · `stream-decision`
- **Purpose:** read, render, and explore decision traces — make decisions
  understandable to humans.
- **Proposed public API:** query API over traces; render components / CLI;
  export & visualization helpers.
- **Depends on:** `decision-trace-model-v2` (reads a trace source, e.g. the ledger).
- **Target maturity:** **beta**.
- **Expected users:** developers; auditors; end-users inspecting decisions; Studio.

### Tier 2 — Layer services

**`interaction-core-v2`** · Interaction · `stream-*` (both)
- **Purpose:** turn raw messages/evidence into structured `Signal`s for the runtime.
- **Proposed public API:** `ingest(input) → Signal[]`; extractor/adapter registry;
  signal-schema conformance (from the model).
- **Depends on:** `decision-trace-model-v2`.
- **Target maturity:** **beta**.
- **Expected users:** app developers ingesting user/system input; runtime-connected starter.

**`multi-agent-orchestrator-core-v2`** · Execution · `stream-decision`
- **Purpose:** enact finalized decisions across agents, tools, and systems;
  coordinate multi-agent execution (does **not** decide).
- **Proposed public API:** `enact(decision) → executionResult`; agent/tool
  registry; execution results traced back to the ledger.
- **Depends on:** `decision-runtime-core`, `decision-trace-model-v2` (+ ledger for execution traces).
- **Target maturity:** **beta**.
- **Expected users:** developers building agentic execution on accountable decisions.

**`decision-trace-gnn-core-v2`** · Learning · `stream-decision`
- **Purpose:** learn from ledgered traces (graph neural network over decision
  traces) and emit improved signals / metrics back into the loop.
- **Proposed public API:** `train(traceset)`; `infer(context) → signalSuggestions | metrics`;
  model export.
- **Depends on:** `decision-trace-ledger-core-k2`, `decision-trace-model-v2`.
- **Target maturity:** **beta → experimental** (research-facing).
- **Expected users:** ML researchers, data scientists, advanced integrators.

### Tier 3 — Design & simulation

**`decision-trace-studio-v2`** · Design · `stream-*` (both)
- **Purpose:** design, simulate, and improve decision systems end-to-end.
- **Proposed public API:** authoring/config API; simulation runner over
  runtime + ledger; scenario management; visualization (via the view).
- **Depends on:** `decision-trace-model-v2`, `decision-runtime-core`,
  `decision-trace-ledger-core-k2`, `dtm-view-core-v2`.
- **Target maturity:** **beta**.
- **Expected users:** system designers, architects, researchers.

### Tier 4 — Entry points (supported on-ramps)

**`light-dtm-starter-kit-cs`** · Entry · `starter-kit` · `stream-decision`
- **Purpose:** minimal, **standalone/local** decision flow — no external services.
- **Proposed public API:** runnable app/CLI + example config; embeds runtime with
  an in-memory/local trace.
- **Depends on:** `decision-runtime-core`, `decision-trace-model-v2`.
- **Target maturity:** **stable** (an on-ramp must be reliable).
- **Expected users:** newcomers, evaluators, learners.

**`light-dtm-starter-kit-cs-v2`** · Entry · `starter-kit` · `stream-decision`
- **Purpose:** the **full** signal → decision → trace flow, runtime-connected.
- **Proposed public API:** runnable reference app wiring interaction → runtime →
  ledger → view.
- **Depends on:** `decision-trace-model-v2`, `decision-runtime-core`,
  `decision-trace-ledger-core-k2`, `interaction-core-v2`, `dtm-view-core-v2`.
- **Target maturity:** **stable** (the recommended first experience).
- **Expected users:** newcomers wanting the whole flow; a reference integration.

### Independent — Front door

**`chinoba-site`** · Website · `stream-*` (both)
- **Purpose:** the chinoba.org front door; docs, narrative, links.
- **Proposed public API:** — (a site, not a library).
- **Depends on:** — (no code dependency on the runtime).
- **Target maturity:** **stable** (production site).
- **Expected users:** everyone. *(Ownership migration only — see roadmap Phase 2.)*

---

## 5 · Dependency graph

```
                        ┌─────────────────────────────┐
                        │  decision-trace-model-v2     │  Tier 0 (keystone)
                        └──────────────┬──────────────┘
        ┌──────────────┬───────────────┼───────────────┬───────────────┐
        ▼              ▼               ▼               ▼               ▼
 decision-       decision-trace-   dtm-view-      interaction-    (studio, below)
 runtime-core    ledger-core-k2    core-v2        core-v2
        │              │               │               │
        │              ▼               │               │
        │      decision-trace-         │               │
        │      gnn-core-v2 (Learning)  │               │
        ▼                              │               │
 multi-agent-orchestrator-core-v2      │               │
 (Execution)                           │               │
        └──────────────┬───────────────┴───────────────┘
                       ▼
        decision-trace-studio-v2   ← depends on model + runtime + ledger + view
                       │
     ┌─────────────────┴───────────────────┐
     ▼                                       ▼
 light-dtm-starter-kit-cs           light-dtm-starter-kit-cs-v2
 (runtime + model)                  (model + runtime + ledger + interaction + view)

 chinoba-site ── independent (front door; no code edges)
```

**Edges (A → B = "A depends on B"):**
- runtime, ledger, view, interaction → **model**
- orchestrator → runtime, model
- gnn → ledger, model
- studio → model, runtime, ledger, view
- starter-cs → runtime, model
- starter-cs-v2 → model, runtime, ledger, interaction, view

No cycles. `model` is the universal sink; the starters are the universal sources.

---

## 6 · Recommended implementation / graduation order

Topologically sorted from the graph, and identical to the sanctioned migration
order in [`../implementation/IMPLEMENTATION_PLAN.md`](../implementation/IMPLEMENTATION_PLAN.md) §6:

1. **`decision-trace-model-v2`** — the shared language; nothing composes without it.
2. **`decision-runtime-core`** — the kernel (make a decision).
3. **`decision-trace-ledger-core-k2`** — record it.
4. **`dtm-view-core-v2`** — see it.
5. **`interaction-core-v2`** — real signals in.
6. **`multi-agent-orchestrator-core-v2`** — act on decisions.
7. **`decision-trace-gnn-core-v2`** — learn from the ledger.
8. **`decision-trace-studio-v2`** — design & simulate whole systems.
9. **`light-dtm-starter-kit-cs`** → **`light-dtm-starter-kit-cs-v2`** — entry points **last** (they depend on the rest).
- **`chinoba-site`** — **independent**; may graduate at any time (ownership-only, no build dependency).

> Each step = *bring to standard on the Bench → graduate (transfer) → assign
> layer team → topic-tag → per-repo branch protection → verify redirect.*
> (Per-repo branch protection substitutes for org rulesets, which are blocked on
> GitHub Free — see the foundation completion report.)

---

## 7 · Minimum Viable Ecosystem (MVE)

**MVE goal:** a newcomer can run the core value proposition — **signal → decision
→ trace** — end to end, locally, with the fewest repos.

**MVE = 4 repositories:**

| Repo | Role in the MVE |
|---|---|
| `decision-trace-model-v2` | the shared language |
| `decision-runtime-core` | makes the decision |
| `decision-trace-ledger-core-k2` | records the trace |
| `light-dtm-starter-kit-cs` | ties them together into a runnable local flow |

That set demonstrates the whole thesis ("AI is decision; decisions are traced")
with no external services.

**Complete First Release (MVE + 3)** — adds the parts that make the flow *real*
and *inspectable*, matching the profile's "recommended" experience:

- `dtm-view-core-v2` (see the trace) · `interaction-core-v2` (real signals) ·
  `light-dtm-starter-kit-cs-v2` (the full runtime-connected flow).

**Deliberately outside the MVE:** Execution (`orchestrator`), Learning (`gnn`),
Design (`studio`) — high value but not required to *show* the core loop; and
`chinoba-site` (front door, ships independently).

---

## 8 · Future / deferred repositories (create only on trigger)

Honoring "no unnecessary repository," these are **planned, not created**:

| Candidate | Purpose | Trigger to create |
|---|---|---|
| `design-notes` | Consolidate the 9-repo design-note cluster into one | If the cluster is merged in-repo (vs. folded into chinoba.org/research) |
| `ecosystem` | Public home for this architecture + roadmap | When these docs should be public |
| Coordination-stream code (`governance`, `multi-agent-coordination`, `runtime-society`) | Currently research themes only | When real code — not research — exists |
| Language client SDKs | Per-language clients over the model/runtime | On demonstrated downstream demand |

---

## 9 · Decisions needed before implementation (for the reviewer)

1. **Primary language & registry.** The `-cs` starter kits suggest **C#/.NET**
   (→ NuGet), while a **GNN** learning repo suggests **Python** (→ PyPI). Confirm:
   is the ecosystem **polyglot** (C# core + Python learning), and what is each
   repo's package registry? This directly affects the `model` package's
   distribution (one canonical schema, per-language bindings?).
2. **Multi-repo is retained.** The existing naming/topic scheme already commits to
   **multi-repo** (transfer-not-copy, topics-not-folders). Recommend confirming
   we keep multi-repo rather than consolidating into a monorepo.
3. **`model-v2` compatibility contract.** As the keystone, its SemVer and
   cross-repo version-negotiation policy must be defined early (a breaking model
   change ripples to all dependents).
4. **Validate the proposed public APIs (§4) against each repo's real code** during
   graduation, and amend this document where they differ (⚠ §0).
5. **MVE scope sign-off.** Confirm the 4-repo MVE (vs. the 7-repo "Complete First
   Release") as the first delivery target.

---

## 10 · Summary

- The ecosystem is the **Decision Stream / five layers**: 10 code repos + the site.
- **Four repos are the reusable core** (`model`, `runtime`, `ledger`, `view`);
  `model-v2` is the keystone everything depends on.
- The dependency graph is **acyclic**, with `model` as the universal foundation
  and the starter kits as the top-level integrators.
- **Build order** follows the graph and the existing migration order.
- **MVE = 4 repos** (model + runtime + ledger + standalone starter) proves the
  signal → decision → trace loop locally.
- **No new core repo is warranted**; coordination-stream code and SDKs are
  future/on-trigger.

> **No repositories are created by this document.** It is a proposal for review.
> On approval, implementation follows §6, one repo at a time, through the
> graduation gate in [`../organization/repository-strategy.md`](../organization/repository-strategy.md).

---

← Back to [Architecture](README.md) · [Repository Map](repository-map.md) ·
[Repository Strategy](../organization/repository-strategy.md) ·
[IMPLEMENTATION_PLAN](../implementation/IMPLEMENTATION_PLAN.md)
