# Organization

*How the Chinoba ecosystem is organized across GitHub.*

This section defines the **structure** of the `chinoba-lab` organization: its
identity, what lives on it versus the personal account, how repositories are
named, and how they move from one surface to the other.

```
Idea → Personal Bench → (graduation) → chinoba-lab Commons
                                              ↓
                                     official, supported,
                                     public Chinoba
```

---

## In this section

| Document | What it defines |
|---|---|
| [`identity.md`](identity.md) | The org's name, handle, brand, and public surfaces |
| [`personal-vs-org.md`](personal-vs-org.md) | The boundary — what belongs on the Bench vs the Commons |
| [`repository-strategy.md`](repository-strategy.md) | How repos are categorized, where they live, and the legacy policy |
| [`naming-conventions.md`](naming-conventions.md) | Repo naming, suffixes, topics, and versioning |
| [`migration-plan.md`](migration-plan.md) | Moving repos from the personal account to the org, safely |

---

## The one rule everything else follows

> A repository lives in **exactly one canonical place**. Personal until it
> graduates; official once it does. We **transfer**, never copy.

GitHub repository transfer preserves stars, issues, history, and — crucially —
installs an automatic redirect from the old URL. That makes graduation a
*move*, not a fork, so no link in the existing documentation ever breaks.

→ Why this mirrors the runtime: [`../architecture/README.md`](../architecture/README.md)

---

← Back to the [chinoba-lab workspace](../../README.md)
