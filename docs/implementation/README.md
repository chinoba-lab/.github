# Implementation

*The phased build plan for the `chinoba-lab` organization, and the execution
guides for each phase.*

These documents are **planning and execution artifacts**. They describe what to
do and in what order; they do not themselves create, transfer, or modify any
repository. Run each phase's steps only when the owner is ready.

---

## What's here

| Document | What it is | Read it when… |
|---|---|---|
| [`IMPLEMENTATION_PLAN.md`](IMPLEMENTATION_PLAN.md) | The master build checklist — status snapshot, review findings, repository strategy, migration safety, and the full phased roadmap (Phases 0–7). | You want the whole picture: what is true today, what moves where, and in what order. |
| [`PHASE_0_SECURITY_CHECKLIST.md`](PHASE_0_SECURITY_CHECKLIST.md) | Phase 0 — Security & governance. The first org settings to apply: require 2FA, restrict repo creation, base permissions, roles, teams, branch-protection baseline, and SECURITY.md guidance. | You're hardening the organization before anything is created or transferred. |
| [`PHASE_1_IMPLEMENTATION_GUIDE.md`](PHASE_1_IMPLEMENTATION_GUIDE.md) | Phase 1 — Organization profile. Step-by-step creation of the one new repo, `chinoba-lab/.github`: the profile, community-health files, templates, labels, release policy, and branch ruleset. | You're giving the org a public face and publishing the default health files. |

---

## Reading order

```
IMPLEMENTATION_PLAN.md          ← start here — the roadmap and the why
  → PHASE_0_SECURITY_CHECKLIST.md   ← do first — org settings & governance
    → PHASE_1_IMPLEMENTATION_GUIDE.md ← then — the .github repo & profile
```

Phase 0 is a prerequisite for Phase 1. Later phases (2–7) are described in
[`IMPLEMENTATION_PLAN.md`](IMPLEMENTATION_PLAN.md) and are **not** started yet.

---

← Back to the [chinoba-lab workspace](../../README.md) · profile source in [`profile/README.md`](../../profile/README.md)
