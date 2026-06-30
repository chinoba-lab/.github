# Repository Map

*Architecture · Chinoba Ecosystem*

The single source of truth for every Chinoba repository **in the official
architecture**: its layer, the stream it serves, where it lives, and its
lifecycle category. Every other document points here rather than restating the
list.

> The personal account also holds **pre-architecture repositories** (earlier
> prototypes and design-note repos) that predate this architecture and are not
> part of the curated set below. Their disposition (archive / merge / retire) is
> handled as a one-time cleanup — see
> [`../../IMPLEMENTATION_PLAN.md`](../../IMPLEMENTATION_PLAN.md) and the
> *Pre-architecture* section near the end of this map.

---

## Official repositories (target: `chinoba-lab`)

| Repository | Layer | Stream | Role | Category |
|---|---|---|---|---|
| `interaction-core-v2` | Interaction | Both | Input → structured signals | Official |
| `decision-runtime-core` | Core | Decision | The Decision OS kernel | Official |
| `decision-trace-model-v2` | Core | Decision | Decision protocol spec | Official |
| `decision-trace-ledger-core-k2` | Core | Decision | Hash-chained ledger | Official |
| `dtm-view-core-v2` | Core | Decision | Trace interface / viewer | Official |
| `multi-agent-orchestrator-core-v2` | Execution | Decision | Enacts finalized decisions | Official |
| `decision-trace-gnn-core-v2` | Learning | Decision | Learns from ledgered traces | Official |
| `decision-trace-studio-v2` | Design | Both | Design & simulate systems | Official |
| `light-dtm-starter-kit-cs-v2` | Entry | Decision | Runtime-connected starter | Official |
| `light-dtm-starter-kit-cs` | Entry | Decision | Standalone / local starter | Official |
| `chinoba-site` | Website | Both | The chinoba.org front door | Official |

---

## Organization meta repositories (target: `chinoba-lab`)

| Repository | Purpose |
|---|---|
| `.github` | Org profile (profile/README.md) + community-health defaults |

→ Profile source: [`../../org-profile/README.md`](../../org-profile/README.md)
→ Community-health files: [`../standards/community-health.md`](../standards/community-health.md)

---

## Legacy repositories (stay on `Masao-Watanabe-AI`, archived)

| Repository | Superseded by | Category |
|---|---|---|
| `decision-trace-model` | `decision-trace-model-v2` | Legacy |
| `interaction-core` | `interaction-core-v2` | Legacy |
| `decision-trace-gnn-core` | `decision-trace-gnn-core-v2` | Legacy |
| `dtm-view-core` | `dtm-view-core-v2` | Legacy |
| `Decision-Trace-Ledger-Core` | `decision-trace-ledger-core-k2` | Legacy |
| `multi-agent-orchestrator-core` | `multi-agent-orchestrator-core-v2` | Legacy |
| `decision-trace-studio` | `decision-trace-studio-v2` | Legacy |

→ Why they stay put: [`../organization/repository-strategy.md`](../organization/repository-strategy.md#legacy-policy--the-repos-that-stay)

---

## Personal-only repositories (stay on `Masao-Watanabe-AI`)

| Repository | Category | Notes |
|---|---|---|
| `masao-watanabe-ai` (profile) | Personal | Describes the person, never moves |
| `chat-ai-platform` (private) | Personal / Product | Adjacent private product — not Chinoba OSS core |
| `Synapse-Insights` | Personal / Product | Adjacent analytics product — keep personal unless it graduates |
| `runtime-chat-experimental` | Sandbox | Experiment on the Bench |

---

## Pre-architecture repositories (disposition pending)

Repos that predate the current five-layer architecture. They are **not part of
the official set** and are not migrated as-is. The disposition for each is
decided in [`../../IMPLEMENTATION_PLAN.md`](../../IMPLEMENTATION_PLAN.md).

| Cluster | Repositories | Proposed disposition |
|---|---|---|
| Superseded prototypes | `decision-trace-engine`, `decision-trace-platform`, `decision-trace-viewer`, `judgment-structure-core`, `Decision-Oriented-Signal-Platform`, `ai-decision-system-map` | Archive on Bench (superseded by the layered architecture) |
| Design-note repos | `time-aware-data-for-ai`, `multi-agent-orchestration-design`, `llm-agent-design-notes`, `user-behavior-event-design`, `decision-metric-design`, `social-context-inference`, `ai-decision-visualization`, `multi-agent-local-value-system`, `decision-pipeline-reference` | Consolidate into one notes repo or fold into chinoba.org/research |

> These are **not** the curated Legacy set (the seven `-v2`/`-core`/`-k2`
> predecessors above). They are an older stratum, surfaced here so the map is
> honest about everything on the personal account.

---

## How to read a repo's place at a glance

On GitHub, the topics carry this whole table:

```
chinoba + layer-<x> + stream-<y>   → an official repo, located in the architecture
starter-kit                         → a supported entry point
(archived flag, no chinoba topic)   → legacy reference
```

→ Topic scheme: [`../organization/naming-conventions.md`](../organization/naming-conventions.md#topics-not-folders)

---

← Back to [Architecture](README.md) · [chinoba-lab workspace](../../README.md)
