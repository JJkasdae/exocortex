# ExoCortex

> Your external cortex: a personal knowledge base that thinks with you and turns ideas into projects — built on [Claude Code](https://claude.com/claude-code) and an [Obsidian](https://obsidian.md) vault.

Notes alone are storage. This project turns a plain-markdown Obsidian vault into a **conversational knowledge partner**: an agent that captures your ideas with discipline, keeps them connected, and — over months and thousands of notes — becomes an *external brain* you can think out loud with, make decisions against, and mine for connections you'd never have found by hand.

The longer you feed it and the more it holds, the more it compounds. Long time spans and large volumes are exactly where it stops being a notebook and becomes an AI companion that consolidates the essence of your personal digital life, knows your context, and accompanies your day-to-day thinking and decisions.

The vault is just markdown files in git. You own it completely, forever, with no lock-in. The intelligence lives in the agent layer on top.

---

## What this is

- **A second brain, not a note dump.** Every capture is decomposed into atomic, single-concept notes linked by `[[wikilinks]]` — so the vault grows as a connected graph, not a pile of documents.
- **A thinking companion.** Ask it what you've learned about X, challenge an idea against your past notes, brainstorm new angles — it grounds every answer in what's actually in *your* vault and cites the notes.
- **Disciplined and auditable.** Nothing changes your vault without your explicit approval. Every modification is one atomic git commit plus a logged reason, so you can always trace *what changed and why*.

## How it works

Two agents, one shared vault:

| Agent | Role | When it runs |
|---|---|---|
| **Scribe** | Your daily companion — discuss, brainstorm, and capture. Modifies the vault only when you explicitly ask to capture something. | Every Claude Code session in this directory. |
| **Gardener** | Periodic maintenance — scans the vault and *proposes* cleanups (merge duplicates, split overloaded notes, surface emerging themes). Read-only: it never writes; you approve each proposal. | On demand, or scheduled. |

Both obey the same rule set ([`.claude/rules/`](.claude/rules/README.md)): one concept = one note ([single source of truth](.claude/rules/single-source-of-truth.md)), relationships expressed only as wikilinks, and every write done as a single [atomic operation](.claude/rules/atomic-operation.md) (edit + log + commit). An optional [semantic search](.claude/skills/rag-search/SKILL.md) layer lets the agent retrieve notes by *meaning*, not just keywords.

## How your knowledge restructures itself

Most tools only ever *add* notes. This vault also **reorganizes itself**. Capture is just the inflow; over time the Gardener proposes three structural moves that keep the graph coherent as it grows — you approve each, nothing is automatic. They are defined in [`.claude/rules/four-actions.md`](.claude/rules/four-actions.md):

- **Fission — split.** A note that quietly grew to cover several distinct concepts gets proposed for splitting into separate atomic notes (each must stand on its own). Keeps "one concept = one note" true as notes accrete.
- **Convergence — merge.** Two notes that turn out to describe the *same* concept get merged — or, if they're merely related, just linked with a `[[wikilink]]`. Kills silent duplication before it spreads.
- **Emergence — discover.** The standout move. When a theme recurs across many notes' *prose* but you have **never named it** — not as a note, not even as a wikilink — the Gardener surfaces it and proposes a brand-new note for the concept you've been circling without realizing. This is where the vault stops mirroring what you typed and starts revealing structure you didn't know was there: the mechanism behind "help me discover viewpoints I never explicitly formed."

**Where to change these behaviors** — the judgment lives in plain editable files; loosen or tighten them and the agents change with them:

| File | Controls |
|---|---|
| [`.claude/rules/four-actions.md`](.claude/rules/four-actions.md) | The definitions of all four actions and the tests each must pass before it can be proposed. |
| [`.claude/rules/substance-test.md`](.claude/rules/substance-test.md) | The bar a concept must clear to deserve its own note (used by Fission and Emergence). |
| [`.claude/agents/gardener.md`](.claude/agents/gardener.md) | *How* the Gardener detects them — e.g. the Emergence "negative-space sweep" and its ≥3-notes threshold. |

## Repository layout

Each folder links to its own README (where one exists) — this file is the top-level entry point; they go deeper.

| Path | What lives here |
|---|---|
| [`CLAUDE.md`](CLAUDE.md) | The **Scribe** agent definition + project instructions (loaded by Claude Code automatically). |
| [`Notes/`](Notes/README.md) | Your knowledge notes — flat, one concept per file. *Starts empty.* |
| `Streams/` | Operational documents (daily digests, periodic reports), organized by date — a corpus separate from the concept graph. *Created on first use.* |
| [`Sources/`](Sources/README.md) | Original source files (PDFs, etc.); kept locally, not committed to git. |
| [`Templates/`](Templates/README.md) | Note templates Scribe uses when creating notes. |
| [`.claude/rules/`](.claude/rules/README.md) | The always-loaded judgment rules both agents obey. |
| [`.claude/skills/`](.claude/skills/README.md) | On-demand procedures: semantic search, PDF/YouTube/Notion/Apple Notes import, Obsidian helpers, and scaffolding a proposal into a new project repo. |
| [`.claude/agents/`](.claude/agents/gardener.md) | The **Gardener** sub-agent definition. |
| `.claude/log/` | Append-only JSONL ledger of every vault change — format in [`log-schema.md`](.claude/rules/log-schema.md); personal, not committed. |
| [`user_level_file_template/`](user_level_file_template/README.md) | A template for your personal user-level `CLAUDE.md` profile. |

---

## Setup

### Prerequisites

| Required | Why |
|---|---|
| **[Claude Code](https://docs.claude.com/en/docs/claude-code)** | The agents run inside it. You need a Claude account. |
| **git** | The vault is versioned; every capture is a commit. |

| Optional | Why |
|---|---|
| **[Obsidian](https://obsidian.md)** | The nicest way to *read and edit* the vault (graph view, backlinks). Any markdown editor works. |
| **Python 3.12 + [uv](https://docs.astral.sh/uv/)** | Only for `rag-search` semantic retrieval. Skip it and the agent falls back to plaintext search. |
| **An OpenAI-compatible embeddings API key** | Only for *live* semantic search. The offline `mock` mode works with no key. |

### 1. Clone and open

```bash
git clone https://github.com/JJkasdae/exocortex.git
cd exocortex
claude
```

Then open the same folder as a vault in Obsidian (*Open folder as vault*) if you want the visual editor.

### 2. Make it yours

- **Name your agent.** The title in `CLAUDE.md` reads `[Please rename your agent]` — replace it with whatever you want to call your assistant.
- **Set up your profile.** Copy [`user_level_file_template/CLAUDE.md`](user_level_file_template/CLAUDE.md) to your **user-level** config and fill in who you are and how you like to work — see [its README](user_level_file_template/README.md) for a walkthrough. This file applies to *all* your Claude Code projects, so the agent calibrates to you. Its location depends on your OS:

  | OS | Path |
  |---|---|
  | macOS / Linux | `~/.claude/CLAUDE.md` — `~` is your home folder (e.g. `/Users/yourname`) |
  | Windows | `%USERPROFILE%\.claude\CLAUDE.md` (e.g. `C:\Users\yourname\.claude\CLAUDE.md`) |

  > **Don't confuse the two `.claude` folders.** The one in your **home directory** (above) is the *global, user-level* config that follows you across every project — that's where this profile goes. The `.claude/` folder *inside this repo* is *project-specific* (it holds the agent machinery: rules, skills, sub-agents) — leave it alone. If a home-level `CLAUDE.md` already exists, merge in the parts you want rather than overwriting.

> [!NOTE]
> **Remove this template once setup is done.** After you've copied and personalized your profile, the `user_level_file_template/` folder has served its purpose and is just clutter in your clone. Delete it yourself (`rm -rf user_level_file_template/`), or ask Claude Code to — e.g. *"I've finished setup, remove the user-level template folder."*
>
> **Note for Claude Code:** When asked to remove `user_level_file_template/`, **verify setup is actually complete first — never delete it preemptively.** Setup counts as finished when the user's profile exists *and has been personalized*: `~/.claude/CLAUDE.md` (macOS / Linux) or `%USERPROFILE%\.claude\CLAUDE.md` (Windows) is present **and** no longer contains the template's scaffolding — specifically the `<Your Name>` placeholder in the title or any `> **Guidance:**` notes (a kept example block like `_Example A —_` is a weak hint, not a blocker). If the file is missing or still holds that scaffolding, setup isn't done: say so and **do not delete**. If the check passes, show the user exactly what will be removed and delete the folder **only after they confirm once** (honoring the project's *never delete without explicit permission* rule). Then commit the removal as its own git commit.

### 3. (Optional) Enable semantic search

Semantic search lets the agent find notes by meaning. It's a best-effort enhancement — everything works without it.

> [!IMPORTANT]
> **The virtual environment must be named exactly `.venv`** (with the leading dot). The `rag-search` skill invokes `.venv/bin/python` as a **hardcoded path** — if your environment has any other name, the skill can't find Python and semantic search silently falls back to plaintext. Use the command below as-is.

**Create the environment** with `uv venv .venv`. Each token:

| Token | Meaning |
|---|---|
| `uv` | the package / environment manager |
| `venv` | the subcommand — *create a virtual environment* |
| `.venv` | the directory to create it in — **must be this exact name** so the skill matches it |

```bash
uv venv .venv                       # create the env at ./.venv (the name the skill expects)
uv pip install -r requirements.txt  # install lancedb, openai, python-dotenv into it
cp .env.example .env                # set up the embedder config
```

**Activate it** — optional, since the skill calls `.venv/bin/python` directly; you only need this to run scripts by hand:

```bash
source .venv/bin/activate       # macOS / Linux
.venv\Scripts\Activate.ps1      # Windows — PowerShell
.venv\Scripts\activate.bat      # Windows — cmd.exe
```

Then edit `.env`:
- Leave `EMBED_PROVIDER=mock` to verify the plumbing offline (deterministic, no API calls, **not** real retrieval).
- Set `EMBED_PROVIDER=api` plus `EMBED_BASE_URL` / `EMBED_API_KEY` / `EMBED_MODEL` / `EMBED_DIM` for real semantic search.

The vector store (`.rag-index/`) is a rebuildable, gitignored projection of your notes — build it once via the reindex path in the [rag-search skill](.claude/skills/rag-search/SKILL.md), and refresh it manually after notes change.

> Prefer `pip`? Equivalent: `python -m venv .venv`, then activate (above), then `pip install -r requirements.txt`. Either way, keep the directory named **`.venv`** — that's the path the skill invokes.

### 4. Start using it

Just talk to Claude Code in this directory. It defaults to **discussing** — it won't touch your vault until you explicitly say "capture this" or "save this as a note."

```
You:  I've been thinking about why my agents keep losing context across sessions...
       [a back-and-forth conversation]
You:  Okay, capture that as a note.
Scribe: [proposes a note + how it connects to existing ones → you approve → one commit]
```

To run maintenance: *"run Gardener"* or *"scan the vault for cleanup."* It returns proposals one at a time; nothing changes until you approve each.

---

## Extend it into your own assistant

The vault is the data; the agent layer is yours to shape. You're not limited to what ships here — this is the path from "a note-taking helper" to a private assistant that lives in *your* context and tools.

- **Add your own skills.** The repo ships importers (PDF, YouTube, Notion, Apple Notes) and Obsidian helpers in [`.claude/skills/`](.claude/skills/README.md) — and you can add whatever workflow *you* repeat. **No coding needed:** describe what you want with the [skill-creation guide](.claude/skills/creating-skills.md) and Claude builds it (or use the `claude-skill-creator` skill to scaffold one). New skills load on demand.
- **Connect MCP servers.** Wire in [MCP](https://modelcontextprotocol.io) servers — email, calendar, drive, task trackers, and more — so the assistant reaches beyond the vault into the tools you already live in.
- **Adapt the rules.** The judgment in [`.claude/rules/`](.claude/rules/README.md) is editable. Tune what's worth capturing, how notes link, and how strict it is — make it think the way *you* do.

Because the data is plain notes and the behavior is plain rules, **nothing ties it to one domain.** The same system works as a daily-life companion, a professional's working knowledge base in any industry, or a small business's shared store of digital assets, SOPs, and decisions — you shape it by what you capture and which skills, MCP servers, and rules you add.

Built up over a long horizon and wired into your tools, this is what turns it into a *real* private assistant rather than a static archive.

---

## Design principles

- **You own everything, and nothing is lock-in.** Plain markdown + git. Delete the agent layer tomorrow and your knowledge is still fully yours and fully readable.
- **The agent proposes; you decide.** No silent edits, no auto-merges, no deletions without permission. Every change is logged and reversible.
- **The vault is the source of truth.** The agent grounds its answers in your actual notes and cites them — it doesn't make things up about what you know.

## Status & contributing

This is a personal project, shared openly as a starting point — and it's built to be **extended**. There are two ways to get involved:

- **Make it your own.** Clone it and adapt it to how *you* think — add [skills](.claude/skills/README.md), connect MCP servers, and tune the [rules](.claude/rules/README.md) to your own needs. Start from [Extend it into your own assistant](#extend-it-into-your-own-assistant).
- **Help improve the project.** Contributions are warmly welcome — new skills, MCP integrations, sharper rules, better docs, or just ideas. Open an issue or a pull request and let's make it better together.

Licensed under the [MIT License](LICENSE) — free to use, modify, and share.
