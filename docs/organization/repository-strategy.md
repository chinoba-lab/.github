# Repository Strategy

*Organization · Chinoba Ecosystem*

How every Chinoba repository is categorized, where it lives, and what happens to
the older ones. This is the bridge between today's reality (everything under one
personal account) and the target (official repos on `chinoba-lab`).

---

## The four repository categories

Every repository in the ecosystem falls into exactly one category. The category
decides its surface, its standards, and its lifecycle.

| Category | Surface | Stability | Example |
|---|---|---|---|
| **Official** | `chinoba-lab` | Supported, versioned | `decision-runtime-core` |
| **Incubating** | `Masao-Watanabe-AI` | Provisional, may graduate | a new layer prototype |
| **Sandbox / Experiment** | `Masao-Watanabe-AI` | Throwaway | a research spike |
| **Legacy / Archived** | `Masao-Watanabe-AI` (frozen) | Read-only reference | `decision-trace-model` (v1) |

> **Official** is the only category that carries a promise. The other three are
> all forms of "not yet" — and that honesty is what protects the promise.

---

## Target mapping — current repositories

The existing documentation already maps each current repository to one of the
five implementation layers. The strategy here adds one column: **where it should
live**. All current core repositories **graduate to the organization**.

### Interaction
| Repository | Target | Layer |
|---|---|---|
| `interaction-core-v2` | → `chinoba-lab` (Official) | Interaction |

### Core / Decision Runtime
| Repository | Target | Layer |
|---|---|---|
| `decision-runtime-core` | → `chinoba-lab` (Official) | Core — the kernel |
| `decision-trace-model-v2` | → `chinoba-lab` (Official) | Core — protocol spec |
| `decision-trace-ledger-core-k2` | → `chinoba-lab` (Official) | Core — ledger |
| `dtm-view-core-v2` | → `chinoba-lab` (Official) | Core — trace interface |

### Execution
| Repository | Target | Layer |
|---|---|---|
| `multi-agent-orchestrator-core-v2` | → `chinoba-lab` (Official) | Execution |

### Learning
| Repository | Target | Layer |
|---|---|---|
| `decision-trace-gnn-core-v2` | → `chinoba-lab` (Official) | Learning |

### Design
| Repository | Target | Layer |
|---|---|---|
| `decision-trace-studio-v2` | → `chinoba-lab` (Official) | Design |

### Starter kits (supported entry points)
| Repository | Target | Notes |
|---|---|---|
| `light-dtm-starter-kit-cs-v2` | → `chinoba-lab` (Official) | Runtime-connected — recommended |
| `light-dtm-starter-kit-cs` | → `chinoba-lab` (Official) | Standalone / local |

### Website (front door)
| Repository | Target | Notes |
|---|---|---|
| `chinoba-site` | → `chinoba-lab` (Official) | Source of [chinoba.org](https://chinoba.org); the org's public front door |

The website is the most-visited surface of the whole ecosystem, so it belongs on
the Commons. If chinoba.org is served from GitHub Pages, transfer the repo and
move the custom domain with it; if it is served elsewhere (Vercel/Netlify),
transfer the repo and re-point the deployment integration at the new owner.

### Pre-architecture repositories
The personal account also holds older prototypes and design-note repos that
predate this architecture. They are **not** official and are **not** part of the
curated Legacy set; they are handled by a one-time cleanup (archive / consolidate
/ retire) defined in [`../../IMPLEMENTATION_PLAN.md`](../../IMPLEMENTATION_PLAN.md).

→ A single, complete table of every repo with its stream and owner:
[`../architecture/repository-map.md`](../architecture/repository-map.md)

---

## Legacy policy — the repos that stay

The existing README keeps a `<details>` block of *earlier / superseded*
repositories. These are **Legacy**, and the policy for them is explicit:

> Legacy repositories **stay on the personal account, frozen and archived.**
> They are never deleted and never migrated.

The superseded set (each replaced by its `-v2` / `-core` / `-k2` successor):

- `decision-trace-model` → superseded by `decision-trace-model-v2`
- `interaction-core` → superseded by `interaction-core-v2`
- `decision-trace-gnn-core` → superseded by `decision-trace-gnn-core-v2`
- `dtm-view-core` → superseded by `dtm-view-core-v2`
- `Decision-Trace-Ledger-Core` → superseded by `decision-trace-ledger-core-k2`
- `multi-agent-orchestrator-core` → superseded by `multi-agent-orchestrator-core-v2`
- `decision-trace-studio` → superseded by `decision-trace-studio-v2`

**Why they stay put:**

1. **History must resolve.** These URLs are printed in published books and blog
   posts. A transfer-plus-archive would technically redirect, but freezing them
   in place is the simplest guarantee that nothing breaks.
2. **The Commons should be clean.** The organization should present only the
   current architecture. Hiding v1 inside the org would dilute that signal.
3. **Provenance has value.** Keeping v1 on the author's personal account
   correctly attributes the lineage of the ideas to the person who explored
   them.

Each legacy repo should be marked **Archived** on GitHub and carry a one-line
README banner pointing to its successor on `chinoba-lab`.

---

## The graduation gate

A repository moves from Incubating to Official only when it clears every bar:

- [ ] **Belongs to a layer.** It implements (or specifies) one of the five
      architecture layers, or is a supported starter/example.
- [ ] **Meets the standards.** Layout, docs, and community-health files match
      [`../standards/`](../standards/README.md).
- [ ] **Has an owner.** A maintainer is named in `CODEOWNERS`
      (see [`../governance/teams-and-roles.md`](../governance/teams-and-roles.md)).
- [ ] **Carries a stability promise.** Versioning and a changelog are in place.
- [ ] **Is depended-upon, or meant to be.** It passes the
      ["should a stranger depend on this?"](personal-vs-org.md) test.

Graduation is reviewed and approved like any other accountable decision — and
recorded. → [`../governance/decision-rights.md`](../governance/decision-rights.md)

---

← Back to [Organization](README.md) · [chinoba-lab workspace](../../README.md)
