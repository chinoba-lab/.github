# ADR-002 — Public API Validation Strategy

- **Status:** Proposed
- **Date:** 2026-07-01
- **Deciders:** Chinoba owner + owning layer team (per repo)
- **Relates to:** [`../ECOSYSTEM_ARCHITECTURE_PROPOSAL.md`](../ECOSYSTEM_ARCHITECTURE_PROPOSAL.md) §0, §4, §9.4 · [ADR-001](ADR-001-language-and-registry-strategy.md)

---

## Context

The public APIs in the ecosystem proposal (§4) are **proposed contracts inferred
from the architecture and the naming grammar** — not observed from code. The
source of the ten official repos lives on the founder's personal account and is
**not present in this workspace**, so nobody has yet reconciled the proposed
surface (`decide(...)`, `append(...)`, `ingest(...)`, etc.) against what the repos
actually expose.

Graduating a repo publishes a **stability promise** (per `repository-strategy.md`
and `RELEASING.md`). Promising an API that doesn't match the code — or freezing an
accidental surface — would break that promise on day one. We need a deliberate
step that turns *proposed* contracts into *verified* ones before any promise is made.

---

## Decision

**Validate every repo's real public API against its proposed contract during the
graduation audit, before that repo graduates.** Concretely:

1. The proposal's §4 API entries are **design intent**, explicitly non-binding
   until validated.
2. As part of "bring each official repo to standard on the Bench" (the graduation
   gate), the owning team performs an **API audit**: enumerate the repo's actual
   public surface, compare it to the proposed contract, and record deltas.
3. **Amend the ecosystem proposal (§4)** to match reality — correct signatures,
   add missing surface, remove what doesn't exist.
4. Only after the audit is the repo's public API **frozen and versioned** (a SemVer
   baseline at graduation), establishing the stability promise.
5. No repo graduates with an unvalidated API.

This makes API validation a **named checklist item in the graduation gate**, not
an afterthought.

---

## Options considered

1. **Assume the proposed APIs are correct** and graduate against them — *Rejected.*
   Publishes a promise on inferred, unverified contracts; high risk of drift.
2. **Validate at graduation, amend the proposal, then freeze** — *Chosen.*
   Aligns doc with code exactly when the promise is made; lowest risk.
3. **Rewrite the repos' APIs to match the proposal** — *Rejected as premature.*
   May be worthwhile later, but forcing code to match an inferred doc inverts the
   source of truth; any redesign should be its own decision, not smuggled into
   graduation.

---

## Consequences

**Positive**
- The stability promise rests on **verified** contracts.
- The proposal document converges to ground truth, repo by repo.
- Drift between intent and code is surfaced early, per layer, by the owning team.

**Negative / costs**
- Adds an explicit audit step to each graduation (time per repo).
- The proposal §4 will change as audits complete — it is a living document until
  all repos graduate.

**Neutral**
- Pairs with [ADR-001](ADR-001-language-and-registry-strategy.md): the audit also
  confirms each repo's **language/registry**, not just its API.

---

## Open questions

1. Who **signs off** an API as validated — the owning layer team, the owner, or both?
2. What is the **SemVer baseline** at graduation — `0.x` (beta) or `1.0` (stable)?
   (Ties to the target-maturity column in proposal §4.)
3. Do we need a **machine-checkable** contract (e.g. the model's JSON Schema,
   OpenAPI, or typed interface snapshots) versus a documented one?
4. How are **cross-repo API dependencies** re-validated when the keystone
   `decision-trace-model-v2` changes version?

---

← [ADR index](.) · [Ecosystem Architecture Proposal](../ECOSYSTEM_ARCHITECTURE_PROPOSAL.md)
