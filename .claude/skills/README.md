# `.claude/skills/` — the agent's on-demand toolkit

> Self-contained procedures the agent loads **only when a task calls for them** — importers, search, and Obsidian authoring helpers.

Skills are the counterpart to the [rules](../rules/). Rules are *always* in context and hold judgment; **skills are loaded only when invoked**, matched by their frontmatter `description`. Each skill is one procedure for one kind of task — ingest a PDF, search by meaning, write a `.canvas` file.

Two principles keep them clean:

- **Skills orchestrate; rules judge.** A skill *references* the rules ([`four-actions`](../rules/four-actions.md), [`atomic-operation`](../rules/atomic-operation.md), …) — it never restates them (single source of truth). The importers below don't bypass the vault's discipline: every item they bring in is deduped, mapped to a four-actions verdict, reviewed with you, and applied as a normal atomic operation.
- **Loaded on demand, by description.** Only each skill's `name` + `description` sit in context until it actually fires, so the toolkit can be large without bloating every session. That `description` is what makes a skill trigger at the right moment.

## The skills

Three groups: bring sources in, find and read, author Obsidian content. The **Needs** column flags anything beyond Claude Code itself.

**Ingest external sources into notes** — each runs the four-actions capture flow and commits one atomic operation at a time.

| Skill | Brings in | Needs |
|---|---|---|
| [`pdf-to-notes`](pdf-to-notes/SKILL.md) | A PDF → one source/hub note + atomic concept notes; original stored in `Sources/`. Per-document, repeatable. | — (Claude reads PDFs) |
| [`notion-import`](notion-import/SKILL.md) | Your Notion page tree → vault notes. One-time bulk migration. | connected Notion MCP |
| [`apple-notes-import`](apple-notes-import/SKILL.md) | Your Apple Notes → vault notes. One-time bulk migration. | macOS + `pandoc` |

**Find & read** — pull in content to work with, from the web, a video, or your own vault.

| Skill | Gives you | Needs |
|---|---|---|
| [`rag-search`](rag-search/SKILL.md) | Semantic / fuzzy search over the vault, plus the reindex that keeps the store in sync. Complements plaintext + graph search. | `.venv` ([setup §3](../../README.md)) |
| [`defuddle`](defuddle/SKILL.md) | Clean markdown from a web page (strips nav/clutter) — for reading or analysis. | Defuddle CLI |
| [`youtube-transcript`](youtube-transcript/SKILL.md) | A YouTube video's transcript, corrected & summarized — to discuss or capture. | `uv` |

**Author Obsidian-native content** — produce correct Obsidian-flavored files.

| Skill | Works with | Needs |
|---|---|---|
| [`obsidian-markdown`](obsidian-markdown/SKILL.md) | Obsidian-flavored markdown: wikilinks, callouts, embeds, properties, tags. | — |
| [`obsidian-bases`](obsidian-bases/SKILL.md) | `.base` files — database-like table/card views with filters & formulas. | — |
| [`json-canvas`](json-canvas/SKILL.md) | `.canvas` files — visual canvases, mind maps, flowcharts. | — |
| [`obsidian-cli`](obsidian-cli/SKILL.md) | Vault operations from the CLI (read/create/search notes, tasks, properties); plugin/theme dev. | Obsidian CLI |

**Spin a proposal into a project** — leave the vault behind and stand up a new dev repo.

| Skill | Does | Needs |
|---|---|---|
| [`project-scaffold`](project-scaffold/SKILL.md) | A finished proposal becomes a new standalone working directory (proposal in `docs/`, stub `CLAUDE.md`, copied `settings.json`, `git init`). Writes outside the vault, so it is not a capture. | — |

> Most skills need nothing beyond Claude Code. The exceptions are in the **Needs** column — set up `rag-search` via [setup §3](../../README.md); the rest are only required if you actually invoke that skill.

## Anatomy of a skill

A skill is a folder containing:

- **`SKILL.md`** (required) — frontmatter (`name` + `description`) then the procedure. The `description` decides *when* Claude loads it, so it states both when to use the skill and when **not** to.
- **`scripts/`** (optional) — executables the procedure calls (e.g. `rag-search` ships Python search/reindex scripts).
- **`references/`** (optional) — detailed docs loaded *only when needed*, so heavy reference material stays out of the main `SKILL.md` (e.g. `obsidian-markdown/references/`).

## Add your own

This is the [root README's](../../README.md) "add your own skills" extension point. **You don't have to write code** — describe what you want with the fill-in-the-blanks guide in [`creating-skills.md`](creating-skills.md) and Claude builds the skill for you.

Under the hood, a good skill:

- **Has a sharp `description` — it is the trigger.** Only `name` + `description` sit in context until the skill fires, so it must say both when to use it *and* when not to.
- **Does one job.** If the purpose needs "and" twice, it's probably two skills.
- **Keeps `SKILL.md` lean, heavy detail in `references/`** — progressive disclosure.
- **Orchestrates the [rules](../rules/), never restates them** — single source of truth.
- **Puts mechanics in `scripts/`, judgment in prose** — deterministic work lives in a script the skill calls.
- **Degrades gracefully** — if it needs a tool or key that may be missing, fail clean (see how `rag-search` falls back to plaintext).

> Use the `claude-skill-creator` skill to scaffold one. (This `README.md` and `creating-skills.md` are documentation, not skills — only folders with a `SKILL.md` are loaded.)
