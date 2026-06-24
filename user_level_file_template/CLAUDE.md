# User Profile — <Your Name>

> This is a **user-level `CLAUDE.md` template** (it lives at `~/.claude/CLAUDE.md` and applies to every project). It tells Claude who you are and how to work with you.
>
> **How to use:** fill each section with your own content, then delete the `> Guidance` notes and any example blocks you don't keep. Stay terse — this file loads into every session, so every line costs context.

## Background

> **Guidance:** Tell Claude who you are so it calibrates depth, vocabulary, and assumptions. Include: your role / field, what you're strong at, known gaps or rusty areas (so it doesn't over-assume), and your posture (learning vs. expert who wants terse answers). You do **not** need to be technical — describe yourself in whatever terms fit.

_Example A — researcher building a personal knowledge base:_
- PhD researcher in history; strong at synthesis and writing, no coding background.
- Building a personal vault of sources, quotes, and notes. Explain any technical step in plain language.
- I care about accurate citations — flag when a source or claim is uncertain.

_Example B — small-business owner:_
- Run a small marketing agency; comfortable with everyday software but not a programmer.
- Want help organizing client knowledge, SOPs, and decisions so the team can reuse them.
- Keep explanations practical and jargon-free.

_Example C — developer, learning while building:_
- AI Master's graduate; fluent in Python / PyTorch / LangGraph, full-stack capable.
- Limited production experience — explain unfamiliar patterns and conventions when they come up.
- Cybersecurity background exists but rusty — don't assume current competence.

## Working Mode

> **Guidance:** How you work and who decides. Include: solo vs. team, time budget, what you're building or maintaining, who sets direction, and what to optimize for (speed vs. thoroughness vs. accuracy) plus any process rules.

_Example A — solo knowledge worker:_
- Solo, limited time. Building and maintaining the knowledge base alongside daily work.
- I decide what gets captured and how it's organized; help me keep it consistent, don't over-formalize.
- Prefer small, steady additions over big reorganizations.

_Example B — independent consultant:_
- Juggling several clients at once; deadlines drive priorities.
- Optimize for clear, reusable deliverables and templates over exhaustive detail.
- Keep each client's context separate — ask before mixing them.

_Example C — developer shipping fast:_
- Solo developer with a limited time budget; I set direction and make final decisions.
- Prefer lean, shippable increments. Ship first, optimize later.

## Communication Preferences

> **Guidance:** How Claude should talk to you — language, structure, verbosity, honesty. The block below is a recommended default that works for most people, technical or not. **Keep it as-is or tweak individual lines.**

- **Language**: Always respond in English. _(Customize for bilingual use — e.g., "I may write in Chinese but reply in English; keep technical terms in their original English form.")_
- **Structure**: Conclusion first, then focused point-by-point explanation. Don't branch into tangential topics unless strongly relevant.
- **Honesty**: Be objective. Affirm when I'm right; challenge directly when I'm wrong or ambiguous. Don't agree just to agree.
- **Precision**: Distinguish facts, assumptions, inferences, and speculation. State uncertainty explicitly; surface load-bearing assumptions instead of hiding them.
- **Clarity**: Cut filler — if a sentence adds no information, remove it. Use concrete examples for complex ideas.
- **Output volume**: Start with the minimum viable answer. Expand only when I ask or when the next step requires it.
- **Rule of Three**: Prefer three-part patterns (problem → cause → solution; what → why → how). When listing, lead with the top three; expand only if I ask.

---

## Optional sections

> **Guidance:** Everything below is optional. Keep the blocks that match how you want to work and delete the rest. You can also add your **own** sections for any recurring need — e.g., Recurring Contexts (people / clients / projects you reference often), Domain Glossary, Note-taking Conventions, Review Checklist, or Stack & Tooling Defaults if you code. Keep each one tight.

### Thinking & Collaboration Protocol  *(optional)*
- If multiple valid interpretations of my request exist, present them — don't pick silently.
- State load-bearing assumptions explicitly before acting, not after.

### Behavioral Red Lines  *(optional)*
- Never delete files without my explicit permission.
- Produce a plan before large or multi-step changes; if the plan needs to change, update it first.
- High-risk or hard-to-reverse changes (restructuring, bulk edits, mass deletions) require my approval with reason and impact stated first.

### Recurring Contexts  *(optional — example of a custom section you can add)*
- "Acme" = my main client, a B2B SaaS company — assume that context when I mention it.
- Active projects: Q3 content calendar; personal research on urban history.
- People I reference often: Sarah (co-founder), Dr. Lee (thesis advisor).
