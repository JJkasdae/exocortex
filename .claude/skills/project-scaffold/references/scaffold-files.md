# Scaffold file contents

Exact contents of each file `project-scaffold` writes into `<target>/`. Substitute `<...>` placeholders at
write time. Keep everything minimal and stack-agnostic — the project fills in specifics later, in its own session.

Write all prose plainly: no em/en dashes, no inflated vocabulary, straight quotes.

---

## `docs/PROPOSAL.md`

The proposal as developed with the user. Frame it with a title and a date, then the proposal body verbatim
from the conversation (and any `Drafts/` staging file). Do not rewrite or pad the user's substance — agent ≠ author.

```markdown
# <Project Name> — Proposal

> Captured <YYYY-MM-DD>. Source of truth for what this project is and why.

<the proposal body as developed with the user>
```

---

## `CLAUDE.md` (lean, structured)

Write it **last**, after any seeding, so the navigation map is accurate. Keep it lean: a short description, a
navigation map of what actually exists, and only rules genuinely known from the proposal/context. Omit empty
sections rather than padding them.

```markdown
# <Project Name>

## What this is
<2-4 lines: the project's purpose and goal, from the proposal>. Core context lives in `docs/PROPOSAL.md`
(source of truth, read first).

## Navigation map
- `docs/` — all document-type material; `docs/PROPOSAL.md` is the source of truth.
- `<seeded data dir>/` — <what it is and its purpose>. <If upstream-maintained: synced copy, source of truth is
  <where>, re-synced manually; treat as read-only reference.>
- `.claude/skills/` — project tools (list each seeded skill + one line; omit if none seeded).
- `.claude/settings.json` — permissions (acceptEdits + deny guards), carried from the vault.

## Conventions
- Document-type files live under `docs/`.
- <only rules genuinely known from context; otherwise leave this for the project's own session>

## Planned (not yet built)
- <known intended work/skills; omit the whole section if none>

## Status
Fresh scaffold. The above is the known starting set; refine it here as the project matures. No stack chosen yet,
so no build/test commands; add a `Commands` section when they exist.
```

---

## `README.md` (stub)

```markdown
# <Project Name>

<one-line description>.

Early-stage project. See [`docs/PROPOSAL.md`](docs/PROPOSAL.md) for the full proposal and rationale.
```

---

## `.gitignore` (universal starter)

Stack-agnostic. The project tightens this once its stack is chosen.

```gitignore
# OS / editor
.DS_Store
*.swp
.idea/
.vscode/

# Environment / secrets
.env
.env.*
!.env.example

# Dependencies / build (common)
node_modules/
__pycache__/
*.py[cod]
.venv/
venv/
dist/
build/

# Logs
*.log

# Claude Code — personal/session files
.claude/settings.local.json
```

---

## `.claude/settings.json`

Not templated. Copy the vault's `.claude/settings.json` byte-for-byte so the new project inherits the same
acceptEdits mode and deny guards (no `rm -rf`, no force push, no hard reset). If the vault's settings change,
new scaffolds track them automatically.
