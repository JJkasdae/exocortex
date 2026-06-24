# `.claude/rules/` — the agent's judgment layer

> The always-loaded rules that decide **what** the agents do and **how** every change lands. Edit these to change how your assistant thinks.

These eight markdown files are the agents' *constitution*. Unlike [skills](../skills/) (on-demand procedures), the rules are **always in context** — present in every [Scribe](../../CLAUDE.md) session, and the [Gardener](../agents/gardener.md) sub-agent auto-loads the subset its read-only role needs. So every capture, link, and cleanup is judged against them.

They hold **judgment criteria, not step-by-step procedures** — *should this become a note? is this a duplicate? how is a change recorded?* Behavior follows the files: change a rule and the agents change with it, no code to touch.

> This `README.md` is documentation for humans — it is **not** itself a rule and is **not** loaded into the agents' context. Only the files listed below are.

## The files

Three groups: what to decide, how to keep the graph clean, how a change lands.

**Judgment core — decide what to do**

| File | Governs |
|---|---|
| [`substance-test.md`](substance-test.md) | The single criterion — does a concept have enough room to grow to deserve its own note or link? Two thresholds: phrase-level (links) and concept-level (notes). |
| [`four-actions.md`](four-actions.md) | The taxonomy every vault change maps to — Assimilation/Creation (capture), Fission, Convergence, Emergence — and the test each must pass. |
| [`content-placement.md`](content-placement.md) | The two corpora — concept notes (`Notes/`, flat, linked) vs operational streams (`Streams/`, by date) — and which one a piece of content belongs to. |

**Relationship integrity — keep the graph clean**

| File | Governs |
|---|---|
| [`linking.md`](linking.md) | When to wikilink a term (phrase-level substance test) versus leave it plain text. |
| [`single-source-of-truth.md`](single-source-of-truth.md) | Detecting and resolving duplicate / conflicting representations *before* writing. Drives Convergence. |

**Write discipline — how a change lands**

| File | Governs |
|---|---|
| [`atomic-operation.md`](atomic-operation.md) | The non-negotiable 3-step write protocol — edit → log → commit, all in one turn. |
| [`log-schema.md`](log-schema.md) | The JSONL ledger format every change is recorded in. |
| [`templates.md`](templates.md) | Template selection when a new note is created. |

## How they fit together

`substance-test.md` is the **foundation** — one criterion reused everywhere. `four-actions.md` is the **spine** — it routes every change to an action and leans on the substance test for the hard calls. The rest are **specifics the spine delegates to**: `linking.md` and `single-source-of-truth.md` keep relationships clean; `atomic-operation.md`, `log-schema.md`, and `templates.md` govern the mechanics of writing a change down.

One discipline runs through all of them — the same single source of truth the vault enforces, applied to the rules themselves: **each rule is stated in exactly one file; the others reference it.** (The "use wikilinks" *principle* lives in [`CLAUDE.md`](../../CLAUDE.md); the *criterion* in `substance-test.md`; the *link specifics* in `linking.md`.) Respect this when editing — change the one authoritative file, never a copy.

## A newcomer's reading order

1. [`substance-test.md`](substance-test.md) — the criterion everything else uses.
2. [`four-actions.md`](four-actions.md) — the taxonomy that organizes all changes.
3. [`atomic-operation.md`](atomic-operation.md) + [`log-schema.md`](log-schema.md) — how a change is executed and recorded.
4. The rest as needed — `linking.md`, `single-source-of-truth.md`, `templates.md`.

## Customizing

These files *are* the customization surface — the [root README](../../README.md) points here for "where to change the agents' behavior." To tune the assistant to how you think:

- **Edit the authoritative file, not a restatement elsewhere** (single source of truth — see above). Some behaviors are split by design: changing how aggressively **Emergence** fires lives in both `four-actions.md` (the test) and [`gardener.md`](../agents/gardener.md) (the ≥3-notes negative-space sweep).
- **Keep them terse.** Rules load into *every* session, so every line costs context. Add judgment, not prose.
- **Preserve `agent ≠ author`.** The rules deliberately make the agent *propose* drafts while *you* write the final wording. Loosening this trades your voice for speed — do it knowingly.

> A rule change takes effect on the **next** session (context is rebuilt at startup). Test a tweak by running a capture or a Gardener scan and checking the result against your intent.
