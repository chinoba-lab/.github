# chinoba-lab — Organization Architecture

*The official GitHub Organization for Chinoba.*

[chinoba.org](https://chinoba.org)

---

## What this workspace is

This directory is the **architecture and operating manual for the `chinoba-lab`
GitHub Organization** — the official open-source home of Chinoba.

It does **not** restate the research. The philosophy, the research streams, and
the five implementation layers are already defined in the existing Chinoba
documentation, and that work stands unchanged. This workspace **inherits** that
design and answers a different, newer question:

> Chinoba has outgrown a single personal account. How should it live across a
> **personal account** and an **official organization** — without losing the
> philosophy that made it coherent?

The existing docs describe *what Chinoba is*. This workspace describes *where
Chinoba's code lives, who owns it, and how it moves there* — the **relationship
between the surfaces that host the work**, designed in the same spirit as the
research itself.

> **Intelligence is relational.** So is a healthy open-source ecosystem.
> `chinoba-lab` is the infrastructure of the relationship between an
> experiment, the people who steward it, and the public that depends on it.

---

## The two surfaces

Chinoba now lives on two GitHub surfaces, with one boundary between them.

| Surface | Handle | Role | One-line |
|---|---|---|---|
| **Personal** | [`Masao-Watanabe-AI`](https://github.com/masao-watanabe-ai) | The Bench | Where ideas begin — profile, experiments, prototypes, sandbox |
| **Organization** | [`chinoba-lab`](https://github.com/chinoba-lab) | The Commons | Where the ecosystem ships — official, supported, public Chinoba |

A repository is born on **the Bench** and, once it earns it, **graduates** to
**the Commons**. That movement is itself a *Signal → Decision* — the same
transformation at the center of Chinoba, applied to the ecosystem that builds
it.

→ The full boundary: [`docs/organization/personal-vs-org.md`](docs/organization/personal-vs-org.md)

---

## How to read these docs

Hub-and-spoke, like the existing Chinoba documentation. Start here, then follow
the spoke you need.

```
chinoba-lab/
├── README.md ····················· you are here — the hub
├── profile/
│   └── README.md ················· the public profile for chinoba-lab/.github
└── docs/
    ├── implementation/ ··········· the phased build plan & execution guides
    │   ├── README.md ············· index of the implementation docs
    │   ├── IMPLEMENTATION_PLAN.md  the master build checklist (review & roadmap)
    │   ├── PHASE_0_SECURITY_CHECKLIST.md  first org settings (security & governance)
    │   └── PHASE_1_IMPLEMENTATION_GUIDE.md  the .github repo, profile, health & templates
    ├── organization/ ············· identity, what lives where, naming, migration
    ├── architecture/ ············· how the org maps to the 5 layers & 2 streams
    ├── governance/ ··············· teams, roles, who decides what
    └── standards/ ················ documentation, repo template, community health
```

| Section | Read it when you want to know… |
|---|---|
| [`docs/organization/`](docs/organization/) | What the org is, what belongs on it, how repos are named and moved |
| [`docs/architecture/`](docs/architecture/) | How every repo maps onto Chinoba's layers and streams |
| [`docs/governance/`](docs/governance/) | Who is responsible for what, and how decisions are made |
| [`docs/standards/`](docs/standards/) | The standards every official repo must meet |
| [`docs/implementation/`](docs/implementation/) | The phased build plan and the Phase 0–1 execution guides |
| [`profile/`](profile/) | The text that introduces the org to the world |
| [`docs/implementation/IMPLEMENTATION_PLAN.md`](docs/implementation/IMPLEMENTATION_PLAN.md) | The master checklist: review findings, migration order, phased roadmap |
| [`docs/implementation/PHASE_0_SECURITY_CHECKLIST.md`](docs/implementation/PHASE_0_SECURITY_CHECKLIST.md) | The first org settings to apply — 2FA, repo-creation policy, roles, SECURITY.md |
| [`docs/implementation/PHASE_1_IMPLEMENTATION_GUIDE.md`](docs/implementation/PHASE_1_IMPLEMENTATION_GUIDE.md) | Phase 1 execution: the `.github` repo, profile, community health, templates, branch rules |

---

## The four principles of this workspace

The research has four principles. The organization that hosts it has four of
its own — each one an application of the same philosophy to the act of
maintaining the ecosystem.

- **One philosophy, two surfaces.** The Bench and the Commons are not two
  projects. They are one ecosystem at two stages of trust.
- **Graduation, not duplication.** Official status is *earned and moved*, never
  copied. A repo lives in exactly one canonical place.
- **The org is the only authority of record.** Just as the runtime is the only
  decision authority, `chinoba-lab` is the only authority for what is *official*
  Chinoba. Everything else is a signal toward that decision.
- **Inherit, never replace.** Every standard here descends from the existing
  Chinoba documentation. New structure, same soul.

---

## Relationship to the existing documentation

The existing `README.md` and `docs/` in the parent repository are **reference
material**. This workspace links *to* them and reasons *from* them, but never
edits or supersedes them. Where this workspace defines something new (a
repository strategy, a governance model, a migration plan), it is **additive**:
it describes how to organize the same ecosystem on a new surface.

→ Existing philosophy: [`../docs/philosophy/`](../docs/philosophy/)
→ Existing architecture: [`../docs/architecture/`](../docs/architecture/)
→ Existing ecosystem overview: [`../README.md`](../README.md)

---

**Chinoba** · *Intelligence as Relationship* · `chinoba-lab`
