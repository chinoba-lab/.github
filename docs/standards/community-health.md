# Community Health

*Standards · Chinoba Ecosystem*

The org-level files that make `chinoba-lab` a safe, legible place to contribute.
GitHub lets these live **once** in the special `.github` repository and inherit
across every repo that lacks its own — so the org defines them in one place.

---

## The `.github` repository

`chinoba-lab/.github` is the org's meta repository. It holds two things:

1. **The public profile** — `profile/README.md`, the org's front page on GitHub.
   → Source: [`../../profile/README.md`](../../profile/README.md)
2. **Default community-health files** — inherited by every repo without its own.

```
chinoba-lab/.github/
├── profile/
│   └── README.md          ← org public profile
├── CODE_OF_CONDUCT.md
├── SECURITY.md
├── CONTRIBUTING.md
├── SUPPORT.md
└── .github/
    ├── ISSUE_TEMPLATE/
    └── PULL_REQUEST_TEMPLATE.md
```

---

## Required community-health files

| File | What it promises |
|---|---|
| `CODE_OF_CONDUCT.md` | The relationship standard — how people treat each other here |
| `SECURITY.md` | How to report vulnerabilities, and the response commitment |
| `CONTRIBUTING.md` | How a signal (a contribution) becomes a decision (a merge) |
| `SUPPORT.md` | Where to ask questions; what is and isn't supported |
| `ISSUE_TEMPLATE/` | Structured intake — bug, feature, question |
| `PULL_REQUEST_TEMPLATE.md` | The checklist every change confirms before review |

---

## CONTRIBUTING, in the Chinoba frame

The contribution flow is the governance model, made friendly:

> Open an issue or PR — that is your **signal**. A maintainer reviews it against
> the standards within the repo's **boundaries**. A human **decides**. The merge
> and its discussion are the **trace**.

CONTRIBUTING should say, plainly:

- where work begins (issue first for anything non-trivial);
- the standards a PR must meet (→ [`repository-template.md`](repository-template.md),
  [`documentation-standards.md`](documentation-standards.md));
- who reviews (the owning team, via CODEOWNERS);
- that maintainers may say no, with a reason.

---

## CODE_OF_CONDUCT, in the Chinoba frame

Chinoba's thesis is that intelligence is relational. A code of conduct is the
**minimum maintenance contract for the relationships** the project depends on —
not boilerplate, but the first-class statement that the Commons is a place where
trust can be established, traced, and kept.

---

## SECURITY, in the Chinoba frame

A project built on *accountable decisions* must be accountable about its own
risks. `SECURITY.md` names a private reporting channel and a response
commitment, so that a vulnerability is handled as a traceable decision rather
than an emergency.

---

← Back to [Standards](README.md) · [chinoba-lab workspace](../../README.md)
