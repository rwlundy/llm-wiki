# LLM Wiki

A system for maintaining persistent, structured knowledge across Claude sessions.

---

## What it is

LLM Wiki is a local folder of markdown files paired with a Claude skill that tells Claude how to read, update, and reason about them. Each file is a wiki entry — a topic you want Claude to maintain an accurate, evolving understanding of across multiple sessions.

It is not a notes tool. You can already dump notes into a file and paste them into a conversation. The difference here is **coherence monitoring**: the skill instructs Claude to compare what you're saying in the current session against what it knows about the topic, and to flag when they diverge. If your questions or decisions today appear inconsistent with the understanding that's been established over weeks of prior sessions, Claude will name the discrepancy and ask you about it.

The wiki tracks not just *what is known*, but *how understanding has changed over time* — and makes drift visible before it compounds.

---

## The core insight

Every LLM session starts fresh. You can work extensively with Claude on a complex problem — building up shared understanding, establishing positions, making decisions — and in the next session that context is gone. You can paste in notes, but notes are unstructured. Claude reads them but doesn't reason against them systematically.

The LLM Wiki solves this with two things working together:

1. **A structured schema** that forces each entry to record not just the current understanding, but its confidence level, its revision history, and what is still uncertain. Claude knows where to look for what.

2. **A skill that monitors coherence** — comparing new inputs against stored entries, and flagging when they diverge. This turns the wiki from a passive reference into an active epistemic partner.

The term for what this tracks is **epistemic drift**: the gradual, often unnoticed shift in how you understand a topic. Decisions made in isolation, assumptions revised without being recorded, framing that subtly changes session by session — these accumulate. The wiki makes that drift auditable and controllable.

---

## Why markdown

The wiki deliberately does not use a vector database, embeddings, or any external service. It is plain markdown files on your local disk.

**Privacy-first by design.** Your wiki content never leaves your machine. The skill file and schema are public (that's this repo); your knowledge store is private. You do not need an API key, a cloud account, or a data processing agreement.

**Human-readable and human-editable.** Every wiki file can be opened in any text editor and read without documentation. You can add a note, correct a sentence, or rearrange content — Claude is designed to tolerate minor human edits gracefully. The schema uses plain `##` headers rather than YAML frontmatter precisely because headers don't break when a non-technical user touches them.

**No dependencies.** The system works with local files and Claude only. There is nothing to install, configure, or maintain.

---

## How to set it up

### 1. Install the skill

Copy the `skill/` folder from this repo into your Claude skills directory:

```
~/.claude/skills/llm-wiki/
```

So the file path is: `~/.claude/skills/llm-wiki/SKILL.md`

The skill will be available in Claude Code as `/llm-wiki`.

### 2. Create your wiki folder

Create a folder anywhere on your local disk:

```
mkdir ~/wiki
```

You can name it anything and put it anywhere. You will tell Claude the path when you start a session.

### 3. Create your first entry

Create a markdown file in your wiki folder using the schema in [`schema.md`](schema.md). The minimum viable entry is:

```markdown
# My Topic

## Current State
What is currently understood about this topic.

## Confidence
**Level:** Medium
**Reason:** Initial understanding, not yet validated.

## Last Updated
**Date:** 2025-01-01
**Trigger:** Initial entry.

## Update Count
1

## Revision Log
*(newest at top)*

### 2025-01-01 — Initial entry
Created.
```

### 4. Create `_index.md`

Create a `_index.md` file at the root of your wiki folder. See [`schema.md`](schema.md) for the structure, or copy and adapt the demo at [`demo/_index.md`](demo/_index.md).

### 5. Start a session

In Claude Code, invoke the skill and tell it where your wiki lives:

```
/llm-wiki My wiki is at ~/wiki
```

Claude will read `_index.md`, load relevant entries, and orient you on what's there.

---

## Folder structure

```
your-wiki/              ← lives on local disk, not in this repo
├── _index.md           ← Claude reads this first; the map
├── topic-one.md
├── topic-two.md
└── ...

llm-wiki/               ← this repo
├── README.md
├── schema.md           ← schema reference
├── skill/
│   └── SKILL.md        ← install to ~/.claude/skills/llm-wiki/
└── demo/
    ├── _index.md
    ├── product-vision.md
    ├── target-persona.md
    ├── core-feature-set.md
    ├── market-assumptions.md
    └── open-questions.md
```

---

## Schema quick reference

| Section | What it contains |
|---|---|
| `## Current State` | Plain-English summary of current understanding. Rewritten on each update. |
| `## Confidence` | High / Medium / Low + one-line reason. |
| `## Last Updated` | Date + one-line description of what triggered the update. |
| `## Update Count` | Integer. Increments with each revision. Used to surface hot topics in the index. |
| `## Open Questions` | Flagged uncertainties. Removed when resolved; resolution noted in revision log. |
| `## Human Notes` | Your section. Claude reads it, never overwrites it. |
| `## Revision Log` | Dated, append-only history. Newest at top. Never overwritten. |

Full schema documentation: [`schema.md`](schema.md)

---

## Demo

The `demo/` folder contains a fictional wiki for a SaaS product called Meridian — a B2B async collaboration tool. The demo illustrates the schema across five entries with different update histories:

- **Product Vision** — stable, high confidence, no revisions
- **Target Persona** — one revision showing a buyer discovery that overturned the initial assumption
- **Core Feature Set** — scope drift across three revisions; integrations added and then removed
- **Market Assumptions** — hot topic; competitive framing shifted twice; confidence deliberately Medium
- **Open Questions** — a standalone entry tracking unresolved strategic unknowns

Read the demo to get a feel for how entries age over time, what drift looks like in a revision log, and how the index surfaces what matters.

---

## Reading your wiki

The default reading experience is any text editor. This is functional. If you want something better:

**Obsidian** (recommended, zero configuration) — open your wiki folder as an Obsidian vault. It renders markdown, provides folder navigation, and offers a graph view of connections. One caveat: avoid using Obsidian's `[[wikilinks]]` syntax in your entries, as Claude will interpret those as literal text rather than links.

**Docsify** (browser-based, no build step) — install with `npm install -g docsify-cli`, then run `docsify serve ~/wiki` to view your wiki in a browser. Useful for sharing or presenting.

The revision log is deliberately a flat append-only list — auditable but not visually scannable for drift. A richer diff view is out of scope for this version. If tracking drift visually matters to you, a `git init` inside your wiki folder and periodic commits gives you a full diff history without any additional tooling.

---

## Backing up your wiki

Your wiki files are the valuable part of this system. Back them up.

- **iCloud / Dropbox** — put your wiki folder inside a synced directory.
- **Time Machine** — if you're on Mac, it will be backed up automatically.
- **Private Git repo** — `git init` in your wiki folder, push to a private repo. This also gives you a full history of every change Claude has made.

The public repo (this one) contains no wiki content — only the skill, schema, and demo files.
