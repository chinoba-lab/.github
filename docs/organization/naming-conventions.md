# Naming Conventions

*Organization · Chinoba Ecosystem*

Names are the cheapest documentation there is. The existing repositories already
follow a consistent, legible scheme; this document makes that scheme **explicit
and binding** for the organization, so the Commons reads as one coherent system.

---

## The grammar of a repository name

```
<domain>-<role>[-<generation>]
```

All lowercase, hyphen-separated (kebab-case). Read left to right:

| Segment | Meaning | Examples |
|---|---|---|
| **domain** | What part of Chinoba this is | `decision-trace`, `interaction`, `multi-agent` |
| **role** | What kind of artifact it is | `core`, `model`, `ledger`, `view`, `orchestrator`, `studio`, `starter-kit` |
| **generation** | Which iteration is canonical | `-v2`, `-k2` |

Worked examples from the existing ecosystem:

- `decision-runtime-core` → the **runtime** *kernel*.
- `decision-trace-model-v2` → the trace **protocol model**, generation 2.
- `decision-trace-ledger-core-k2` → the trace **ledger** kernel, ledger-gen K2.
- `dtm-view-core-v2` → the **view** kernel, generation 2.
- `multi-agent-orchestrator-core-v2` → the multi-agent **orchestrator** kernel, gen 2.

> Do not invent a new name shape for a new repo. Find the existing repo nearest
> to it and follow its grammar. Consistency is the feature.

---

## Role vocabulary

A small, fixed vocabulary keeps names predictable. Prefer these roles:

| Role | Use for |
|---|---|
| `core` | A kernel / primary library — the load-bearing implementation of a layer |
| `model` | A specification or protocol definition |
| `ledger` | Append-only, traceable record components |
| `view` | Interfaces that visualize or explore |
| `orchestrator` | Execution / coordination components |
| `studio` | Design, simulation, and authoring tools |
| `starter-kit` | A runnable entry point for newcomers |

If a new artifact does not fit an existing role, that is a signal to discuss it
in governance before minting a new role — not to coin one ad hoc.

---

## Generation suffixes

- **No suffix** — the original / first generation.
- **`-v2`, `-v3`, …** — a successor generation. The suffixed repo is canonical;
  the unsuffixed one becomes [Legacy](repository-strategy.md).
- **`-k2`** — a domain-specific generation marker (the ledger line). Preserve
  existing markers exactly; they are part of the published record.

> A new generation **adds a repo**, it does not rename the old one. The old name
> keeps resolving forever; the new suffix tells you which to build against.

---

## Topics, not folders

GitHub organizations have a flat namespace — there are no nested directories for
repos. The five-layer structure is expressed with **repository topics**, applied
uniformly across the org:

| Topic | Applied to |
|---|---|
| `chinoba` | Every official repo (the umbrella tag) |
| `layer-interaction` | Interaction-layer repos |
| `layer-core` | Core / Decision Runtime repos |
| `layer-execution` | Execution-layer repos |
| `layer-learning` | Learning-layer repos |
| `layer-design` | Design-layer repos |
| `stream-decision` / `stream-coordination` | Which research stream it serves |
| `starter-kit` | Supported entry points |

Topics let anyone reconstruct the architecture from a search, with no folder
hierarchy to maintain. → [`../architecture/repository-map.md`](../architecture/repository-map.md)

---

## Casing note

The existing ecosystem contains one repo named with mixed case
(`Decision-Trace-Ledger-Core`, a v1 that is now Legacy). Going forward, **all
new repos are lowercase-kebab-case.** The mixed-case legacy name is preserved
as-is — never rename a published URL.

---

← Back to [Organization](README.md) · [chinoba-lab workspace](../../README.md)
