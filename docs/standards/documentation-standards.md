# Documentation Standards

*Standards · Chinoba Ecosystem*

The existing Chinoba documentation has a recognizable voice and a consistent
shape. This document turns that observed style into **explicit rules**, so every
official repo reads as one project. Nothing here is invented — it is the
existing docs, described.

---

## Voice

- **Declarative and relational.** State what is true. Frame ideas in terms of
  relationships — between AI, humans, organizations, society — because that is
  the philosophy.
- **Em-dash cadence.** The existing prose uses the em-dash to extend and qualify
  a thought — like this. Keep that rhythm; it is part of the brand.
- **One load-bearing idea per blockquote.** Reserve `>` blockquotes for the
  single principle a section turns on. Don't dilute them.
- **No hype.** Capability claims are quiet and precise. The drama lives in the
  ideas, not the adjectives.

---

## Structure of a document

Every standalone doc follows the existing theme-doc pattern:

```markdown
# Title

*Section · Chinoba Ecosystem*        ← italic tag line, who/where this belongs

Opening sentence stating the one thing this document is about.

---

## Sections with -- tables, prose, and a blockquote principle --

> The principle this document turns on.

---

← Back to [Parent](README.md) · [chinoba-lab workspace](../../README.md)
```

Required elements:

| Element | Rule |
|---|---|
| **Title** | `#` H1, plain noun phrase |
| **Tag line** | `*Section · Chinoba Ecosystem*` in italics, right under the title |
| **Opening line** | One sentence naming the document's subject |
| **Back-link** | End with `← Back to …`, using the `←` arrow |
| **Pointers** | Use `→` for "see also" / forward references |

---

## Symbols, used consistently

The existing docs use a small symbol vocabulary. Keep it exact:

| Symbol | Meaning |
|---|---|
| `→` | A pointer / link / "leads to" |
| `←` | A back-link to the parent |
| `↓` | A step in the decision flow diagram |
| `>` | A load-bearing principle (blockquote) |
| `·` | A separator in tag lines and footers |

Do **not** introduce emoji into ecosystem/architecture docs. (The archived
marketing README uses 👉/📘 for a pitch deck; that register is intentionally
separate and is not the standard for org documentation.)

---

## Hub-and-spoke organization

Mirror the existing repository's information architecture:

- The root `README.md` is the **hub** — overview, then a table of links out.
- `docs/<topic>/README.md` is each **spoke's** own hub.
- Leaf documents are the spokes' spokes, and always link back up with `←`.

> A reader should be able to start at any `README.md` and reach any document in
> at most a few labeled hops — and always find their way back.

---

## Diagrams

Use fenced code blocks for flow diagrams, in the existing style:

```
Interaction → Signal → Decision (Runtime) → Boundary → Human → ┬ Execution
                                                               └ Log (decision trace)
```

ASCII over images where possible — it diffs, it's accessible, and it matches the
existing docs.

---

## Bilingual material

Books and long-form content are published in **Japanese and English**, as the
existing books doc shows. When a repo's docs include such material, present JP
and EN as peers (not one as a translation afterthought), following the
existing `docs/books` pattern.

---

← Back to [Standards](README.md) · [chinoba-lab workspace](../../README.md)
