# Content Placement

Two kinds of content live in this vault; they obey opposite rules. Decide which a piece of content is **before** deciding where it goes.

## The two kinds

| | Concept notes | Operational documents |
|---|---|---|
| What | A unique idea, learning, project, or reference | One issue of a recurring series (daily digest, periodic report, log) |
| Identity | By meaning ("Tau Scaling Law") | By type + date ("Market digest 2026-06-16") |
| Origin | You author / distill it | Arrives on a schedule (often agent-generated) |
| Graph | A node in the wikilink graph; richly linked | Mostly unlinked; the stream is one concept, the items are not |
| Lifecycle | Permanent; revised in place | Ages out; pruned / archived in batches |
| Governed by | `four-actions`, `single-source-of-truth`, `substance-test`, `atomic-operation` | this file + git history only |

## The test — three questions

1. A unique idea, or one issue of a recurring series?
2. You thought it through and wrote it, or it arrived on a schedule?
3. Keep and revise it, or read it and let it age out?

All "former" → concept note. All "latter" → operational document. Mixed → default to concept note (more reversible); reconsider if volume proves otherwise.

## Placement

- **Concept notes** → flat in `Notes/`, organized by `[[wikilinks]]`, never topic folders. Flatness rule lives in `CLAUDE.md` → Vault principles; growth staging below.
- **Operational documents** → a dedicated top-level directory `Streams/`, sibling to `Notes/`, one sub-folder per stream, items named by date. Never under `Notes/` — that keeps the concept graph clean. Created on first real use, never preemptively; git-tracking (commit vs gitignore) is decided per stream at creation.

## Growth staging (concept notes)

Default is flat; add structure only on real pain, never anticipated. Principle: **good frontmatter > good folder structure > poor naming.**

| Vault size | Action |
|---|---|
| < 50 notes | Stay flat — use Quick Switcher, search, graph, backlinks. |
| 50–150 notes | Add frontmatter `type:` (backwards-compatible; enables Bases views; allows multi-type notes). |
| 150+ **and** real pain | Add minimal lifecycle folders only — `Archive/` for inactive notes (`Templates/` already exists). Concept notes stay flat regardless. |

Re-evaluate when: note count crosses 50 or 150; **or** 3+ concrete pain instances (forgot a note existed, >30s to find a known note, duplicate found late); **or** an SSOT crisis. Until a trigger fires, hold the line on flat.

## Scope boundary (prevents rule collisions)

**SSOT applies project-wide.** Single source of truth — one fact, one representation, referenced not copied — governs every corpus: `Notes/` and `Streams/`. It exists to keep information true and prevent drift and hallucination, so no folder is exempt from it.

**The process machinery stays `Notes/`-only.** The rest of the judgment/ledger rules — `four-actions` classification, `substance-test`, `atomic-operation`, `log-schema` / `changes.jsonl` — apply to **`Notes/` concept notes only**. `Streams/` is exempt from that machinery, **not** from SSOT:
- No four-actions classification and no `changes.jsonl` entry for `Streams/` writes.
- Recurring operational items are not SSOT violations *by shape* — two near-identical daily issues are different-date issues of one series, not the same fact duplicated. SSOT still forbids duplicating a *fact* between a stream item and its canonical note.
- The Gardener scans `Notes/` only and never touches `Streams/` — so `Streams/`-side SSOT is upheld at **write time** by Scribe (and at review), not by an automated sweep.

## The pipeline

Streams are read-material, not knowledge. When an item yields an insight worth keeping, **distill it into a concept note** in `Notes/` via normal capture — the file does not move; the knowledge is extracted and linked. From that point the normal `Notes/` rules govern it.
