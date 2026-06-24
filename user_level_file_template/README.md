# `user_level_file_template/` — your personal profile

> A starting template for your **user-level** `CLAUDE.md` — the profile that tells Claude who you are and how to work with you, across every project.

This folder holds one file: [`CLAUDE.md`](CLAUDE.md), a fill-in template for your personal Claude Code profile. The vault doesn't use it directly — you **copy it to your home directory**, where it shapes how *every* Claude Code session (this project included) talks to you: depth, tone, vocabulary, and what it assumes about you.

## Two `CLAUDE.md` files — don't confuse them

| File | Scope | Holds |
|---|---|---|
| `~/.claude/CLAUDE.md` *(you create it from this template)* | **Global** — every project | Who *you* are and how you like to work. |
| [`CLAUDE.md`](../CLAUDE.md) *(this repo's root)* | **This project** | The Scribe agent's instructions for the vault. |

This template feeds the **global** one. Leave the project file alone.

## How to use it

1. **Copy** `CLAUDE.md` to your user-level location (if one already exists, merge in what you want rather than overwrite):
   - macOS / Linux: `~/.claude/CLAUDE.md`
   - Windows: `%USERPROFILE%\.claude\CLAUDE.md`
2. **Fill each section** in your own words. *Background*, *Working Mode*, and *Communication Preferences* are the core (the Communication block is a sensible default you can keep as-is); the sections below them are optional. Three persona examples — researcher, business owner, developer — are included as starting points: keep the one that fits, rewrite it, or delete it.
3. **Delete the scaffolding** — remove the `> Guidance` notes and any example blocks you didn't keep.

## Why it matters (and why keep it terse)

- **Calibration.** The knowledge base agent reads this to pitch explanations at your level and match your style — it's [setup step 2](../README.md#2-make-it-yours).
- **Every project.** Because it's global, the profile follows you to *all* your Claude Code work, not just this vault.
- **Context cost.** It loads into *every* session, so every line costs context — keep it tight. Say what changes how Claude should respond; cut the rest.

> You don't have to be technical to write a good profile. Describe yourself in whatever terms fit — the examples include non-coding personas on purpose.
