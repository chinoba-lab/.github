# ADR-001 — Language & Registry Strategy

- **Status:** Proposed
- **Date:** 2026-07-01
- **Deciders:** Chinoba owner (org owner)
- **Relates to:** [`../ECOSYSTEM_ARCHITECTURE_PROPOSAL.md`](../ECOSYSTEM_ARCHITECTURE_PROPOSAL.md) §9.1, §2

---

## Context

The ecosystem proposal surfaced an unresolved question: what language(s) and
package registries does the Chinoba code ecosystem target? The existing naming
grammar and repo roles point in more than one direction:

- The starter kits are named `light-dtm-starter-kit-**cs**` — strongly suggesting
  **C#/.NET** (→ NuGet).
- The Learning repo is `decision-trace-**gnn**-core-v2` — graph neural networks
  are overwhelmingly **Python** (PyTorch/DGL/PyG) (→ PyPI).
- Web-facing pieces (the `view`, `studio`, and `chinoba-site`) are naturally
  **TypeScript/JavaScript** (→ npm).

The keystone, `decision-trace-model-v2`, is depended on by **every** repo (see
proposal §2). Its distribution model therefore constrains everything: if the
model is a single-language package, it forces that language on all consumers.

⚠ The actual source of these repos lives on the founder's personal account and is
not in this workspace, so this ADR reasons from the architecture and names, not
from observed code (see [ADR-002](ADR-002-public-api-validation-strategy.md)).

---

## Decision

**Treat Chinoba as an explicitly polyglot ecosystem.** Each repository uses the
language that is *natural to its role*, drawn from a small, fixed set:

| Language | Registry | Natural home |
|---|---|---|
| **TypeScript** | npm | web/interface repos: `dtm-view-core-v2`, `decision-trace-studio-v2`, `chinoba-site` |
| **Python** | PyPI | learning/ML: `decision-trace-gnn-core-v2` |
| **C# / .NET** | NuGet | runtime/kernel & starters where already `-cs`: `light-dtm-starter-kit-cs*`, and runtime/ledger if their real code is .NET |

**`decision-trace-model-v2` is the conceptual source of truth** for the protocol.
It is defined **language-neutrally** (a canonical schema — e.g. JSON Schema — plus
a written spec), and each language consumes it via a thin, conformant binding.
The *concept* is canonical; the *bindings* are generated or hand-maintained to
match it. No single language "owns" the model semantics.

Per-repo language is **confirmed during that repo's graduation audit**
([ADR-002](ADR-002-public-api-validation-strategy.md)), not assumed from its name.

---

## Options considered

1. **Single-language monolith** (pick one language for everything) — *Rejected.*
   Forces GNN work out of Python or web work out of TS; fights each layer's
   natural ecosystem.
2. **Polyglot, language-per-natural-fit, model as neutral source of truth**
   — *Chosen.* Matches the evidence in the names and the roles; keeps the model
   canonical without imposing a language.
3. **One core language + FFI bridges** — *Rejected for now.* Adds binding
   complexity and a runtime coupling the multi-repo design deliberately avoids.

---

## Consequences

**Positive**
- Each layer lives in its most productive ecosystem; contributors use idiomatic tools.
- The model stays the single conceptual authority regardless of consumer language.

**Negative / costs**
- Requires a **cross-language conformance mechanism** for the model (a shared test
  suite / schema fixtures every binding must pass).
- **Per-language CI, versioning, and release** toolchains to maintain.
- Cross-repo version negotiation must be defined at the schema level, not the
  package level (a breaking model change ripples across languages at once).

**Neutral**
- Reinforces the existing multi-repo decision (proposal §9.2); does not reopen it.

---

## Open questions

1. Which artifact is the **canonical** model definition — JSON Schema, a `.proto`,
   or the written spec — and are per-language bindings **generated** or
   **hand-maintained**?
2. Where does the **cross-language conformance test suite** live (in
   `decision-trace-model-v2`, or a separate fixtures set)?
3. Are `decision-runtime-core` and `decision-trace-ledger-core-k2` actually **.NET**
   (as the `-cs` starters imply), or another language? — resolve in ADR-002 audit.
4. Does polyglot imply eventual **language client SDKs** (proposal §8), and if so,
   on what trigger?

---

← [ADR index](.) · [Ecosystem Architecture Proposal](../ECOSYSTEM_ARCHITECTURE_PROPOSAL.md)
