# Open Research

*Governance · Chinoba Ecosystem*

How Chinoba conducts research in the open — and which GitHub feature carries
which part of the dialogue. This document extends
[Decision Rights](decision-rights.md) from *governing the organization* to
*governing the research itself*.

> Communication creates Intelligence. This document is about keeping that
> communication legible.

---

## The principle

Chinoba is both a **Library** — stable knowledge in
[`../research/`](../research/) — and a **Laboratory** — knowledge in motion in
[`../../lab/`](../../lab/). The Laboratory only works if the conversation around
it is public and well-placed. GitHub gives us three surfaces for that
conversation; each carries a different stage of an idea.

→ The philosophy behind this: [`../../OPEN_RESEARCH.md`](../../OPEN_RESEARCH.md)

---

## The three surfaces

| Surface | Carries | Chinoba term |
|---|---|---|
| **GitHub Discussions** | Open-ended thinking — before there is concrete work | The **signal**, forming |
| **Issues** | Concrete, trackable work | The **signal**, specified |
| **Pull Requests** | Change to the repository | The **decision**, enacted |

An idea typically moves left to right — a Discussion sharpens into an Issue,
which is resolved by a Pull Request — but it can loop back at any time.

---

## GitHub Discussions — for thinking

Use Discussions when the question is still open and the answer is not yet
concrete.

- **Questions** — anything you want the community to weigh in on.
- **Brainstorming** — early, speculative ideas.
- **Research discussions** — working through a research question in the open.
- **Architecture debates** — arguing a design *before* it becomes a proposal.

A Discussion worth keeping is captured as a durable record in
[`../../lab/discussions/`](../../lab/discussions/) using the
[discussion template](../../templates/discussion-template.md).

---

## Issues — for concrete work

Use Issues when there is a specific, trackable piece of work.

- **Concrete work** — a defined task with a clear "done."
- **Tasks** — steps toward a larger goal.
- **Bugs** — something that is broken.
- **Feature requests** — a specific capability being asked for.

If an idea is still being *debated*, it belongs in a Discussion, not an Issue.
An Issue is a signal that has been specified enough to act on.

---

## Pull Requests — for change

Use Pull Requests to change the repository.

- **Implementation** — code.
- **Documentation** — notes, READMEs, research pages.
- **Architecture updates** — new or revised designs, and the ADRs that record
  them.

A Pull Request is where the **decision** is enacted and its **trace** recorded,
exactly as in [Decision Rights](decision-rights.md): a PR is a signal, a merge is
a decision, the git history is the ledger.

---

## Choosing the right surface

```
Still thinking, no defined outcome?          → Discussion
Defined outcome, work to be tracked?         → Issue
Ready to change the repository?              → Pull Request
```

When in doubt, start further left. It is cheaper to promote a Discussion into an
Issue than to reopen a debate inside a Pull Request.

---

## Open by Default, Private when Necessary

All three surfaces are public by default. The exceptions are narrow and
deliberate: security reports follow [`../../SECURITY.md`](../../SECURITY.md)
rather than public Issues, and anything touching personal data, a collaborator's
unpublished work, or a confidence given in trust stays out of the public record —
noted as *not stored* where a trace would otherwise sit.

---

← Back to [Governance](README.md) · [The Laboratory](../../lab/README.md)
