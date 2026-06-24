---
name: project-scaffold
description: Scaffold a new standalone development working directory from a project proposal developed with Scribe. Use ONLY on the user's explicit instruction to stand up a working directory for a project (e.g. "create a working directory for this project at <path>", "spin this proposal out into a repo at <path>"). The proposal can come straight from the conversation; no vault note is required. Creates the target dir, writes the proposal to docs/PROPOSAL.md, drops a lean structured CLAUDE.md (high-level description + navigation map) + README + .gitignore, copies the vault's .claude/settings.json, optionally seeds user-named supporting material (skills, reference data/dirs) into the project, then runs git init and an initial commit. Writes ONLY to the target directory OUTSIDE the vault: it is not a vault capture, runs no four-actions judgment, and writes no changes.jsonl entry. Do NOT use for capturing notes, editing the vault, or without an explicit target path and create-project intent.
---

# Project Scaffold

## Overview

Spin a finished project proposal out of the Scribe conversation into a new, standalone development
working directory. The user develops the proposal in chat (optionally staged in `Drafts/`); this skill
materializes it as a self-contained repo skeleton so a fresh Claude Code session can pick it up with the
proposal as its core context.

This skill lays a **skeleton, not a full setup**. It writes a lean, structured `CLAUDE.md` (high-level
description, navigation map, and only rules genuinely known from context) but never copies the vault's
`CLAUDE.md`/`rules/`, never fabricates conventions, and never picks or scaffolds a tech stack. A development
project's deeper conventions differ from the knowledge base's and belong to that project's own session.

**Scope boundary.** Writes only to the target directory, which lives OUTSIDE this vault. It is not a
four-actions capture: no `changes.jsonl` entry, no atomic-operation protocol, no `Notes/` change. (Capturing
the project's high-level idea into `[[Project Ideas]]` is a *separate* normal Scribe capture — see step 6.)

## When to use

Trigger only when ALL hold:

- The user expresses **explicit create-a-working-directory intent** (Manage-mode equivalent for project setup).
- A **target path** is given or asked for (e.g. "...at `~/dev/cookbox`").
- The **proposal context is sufficient** to seed the project — from the conversation and/or a `Drafts/` file. A vault note (`[[Project Ideas]]` or a detail note) is **not** required; a project brainstormed entirely in chat qualifies.

## When NOT to use

- Discussing, refining, or drafting a proposal with no "create the directory" instruction — that is Discuss/Brainstorm.
- Capturing notes or editing the vault — that is Scribe capture / the four actions.
- Any request without a target path or without explicit project-creation intent.

## Input

- **Target path** (required) — where the new working directory goes. Absolute, or `~`-relative.
- **Proposal content** — assembled from the live conversation, and/or a draft file under `Drafts/` if the
  user staged one there. The proposal is whatever the user developed; do not pad or editorialize it
  (agent ≠ author for the substance).
- **Project name** (optional) — defaults to the target directory's basename; confirm it.
- **Supporting material to seed** (optional) — names or paths of existing **skills** and/or **reference
  data/dirs** (e.g. a profile, datasets, docs) the user wants copied into the project (see step 3b).
  User-directed only; nothing is seeded unless asked.

## Procedure

### 1. Assemble the proposal and confirm inputs

- Gather the proposal text from the conversation (and `Drafts/<...>.md` if one was staged). No vault note is
  required — a project brainstormed entirely in chat is a valid, complete source. If the proposal is thin or
  ambiguous, surface that and confirm with the user before scaffolding — a working directory is only as useful
  as the proposal seeding it.
- Confirm: target path, project name (default = basename of target), the one-line description, and any
  supporting material to seed (step 3b).

### 2. Pre-flight guards (stop on any breach, surface to the user)

- **Outside the vault.** The target must not be inside this vault's directory tree. A nested repo inside the
  vault would entangle two git histories. If the path resolves inside the vault, stop and ask for a path outside it.
- **No clobber.** If the target already exists and is non-empty, stop and confirm before writing anything.
- **No double-init.** If the target is already a git repo, skip `git init` (still write any missing scaffold files, with confirmation).

### 3. Create the structure

Layout (see `references/scaffold-files.md` for the exact contents of each generated file):

```
<target>/
├── .claude/
│   ├── settings.json     # copied verbatim from this vault's .claude/settings.json
│   └── skills/           # optional: user-named skills (step 3b)
│       └── <skill-name>/
├── docs/
│   └── PROPOSAL.md       # the proposal (all document-type files live under docs/)
├── <seeded data/dirs>    # optional: user-named reference data, mirrored here (step 3b)
├── .gitignore            # stack-agnostic universal starter
├── CLAUDE.md             # lean structured: description + navigation map + known rules
└── README.md             # one-line description + pointer to the proposal
```

- `mkdir -p <target>/docs <target>/.claude`.
- Write `docs/PROPOSAL.md`, `README.md`, `.gitignore` per `references/scaffold-files.md`.
- Copy this vault's `.claude/settings.json` into `<target>/.claude/settings.json` **verbatim** (do not template it) — this carries the acceptEdits mode and the deny guards so the new project's agent runs without re-prompting.
- Write `CLAUDE.md` **last** (after any step-3b seeding) so its navigation map reflects the final layout, including everything seeded.

### 3b. Optional — seed named supporting material (only on request)

Beyond `settings.json`, a project often needs existing **skills** or **reference data** to work from day one.
When the user **names** material, copy it in. Two kinds:

- **Skills** → `<target>/.claude/skills/<skill-name>/`. Copy the whole folder (`SKILL.md` + any `scripts/`,
  `references/`), preserving structure. Source: a path the user gives, or a skill under this vault's
  `.claude/skills/<name>/` if named.
- **Reference data / context** (files or dirs, e.g. a profile, datasets, docs) → mirrored at the project root
  under its own name by default (so any skill that reads it by relative path keeps working), or a location the
  user specifies.

Guardrails:

- **User-directed only.** Never auto-include anything; never bulk-copy the vault's KB/Obsidian skills — they are
  vault-bound (wikilinks, four-actions, rag-search) and irrelevant to a development project.
- **Skip secrets and junk.** Do not copy `.env`/secret files or VCS/build artifacts (`.git/`, `__pycache__/`,
  `node_modules/`) unless explicitly asked.
- **Record provenance for upstream-maintained data.** If seeded data has a source of truth elsewhere (e.g. a
  reference dataset or corpus maintained outside this project), note in `CLAUDE.md` that the copy is a synced
  snapshot and that re-sync is manual — so the project never mistakes its copy for the master.
- **List what was seeded** in the `CLAUDE.md` navigation map, so the project's own session knows the tools and
  data are there.

### 4. Initialize git

Unless the target is already a repo:

```
git init
git add -A
git commit -m "chore: scaffold <project> from proposal"
```

This is the **new project's own** independent history, unrelated to the vault repo.

### 5. Report

Show the created tree and the single next step: open Claude Code in `<target>`, where it reads
`docs/PROPOSAL.md` as core context. Do not narrate beyond that.

### 6. Optional — capture the high-level idea (separate, vault-side)

`[[Project Ideas]]` is **not** a precondition: many projects are brainstormed entirely in conversation and never
get a note, and the scaffold still runs. Only if the user wants a persistent record, offer once to capture the
high-level idea as a normal Scribe capture (its own atomic operation: edit + `changes.jsonl` line + commit, per
the four actions). This is **not** part of the scaffold — the user decides, and it runs as a distinct Scribe
action against `Notes/`.

## Constraints

- **Skeleton, not full setup** — populate `CLAUDE.md` only from what is genuinely known (proposal + seeded
  layout); never copy the vault's `CLAUDE.md` or `rules/`, never fabricate conventions, never assume or scaffold
  a tech stack (no `package.json`/`pyproject`/language files). Deeper conventions are authored later, in the
  project's own session.
- **Outside-the-vault writes only** — never modify `Notes/`, `Streams/`, or `.claude/log/`; write no
  four-actions log entry. The only vault-side action this skill ever takes is the optional step-6 capture, which
  is an explicit, separate Scribe operation.
- **No clobber, no double-init** — confirm before writing into an existing non-empty directory; do not re-init an existing repo.
- **Seed only on request** — copy skills/data only when the user names them; never auto-include, never bulk-copy the vault's KB/Obsidian skills, never copy secrets.
- **Settings copied, not invented** — `settings.json` is copied byte-for-byte from the vault so guards stay in sync.
