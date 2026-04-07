# LLM Wiki — Schema Reference

This document defines the structure of a wiki entry file. All entries follow the same schema. Sections are identified by their `##` header name — order doesn't matter, and Claude won't break if you add lines, reword content, or rearrange things slightly.

---

## Entry File Template

```markdown
# [Entry Title]

## Current State
Plain-English summary of what is currently understood to be true on this subject.
Write it as if explaining to a thoughtful colleague who is new to the topic.
This section is rewritten (not appended) when updated.

## Confidence
**Level:** High / Medium / Low
**Reason:** One or two sentences explaining the basis for this confidence level.

## Last Updated
**Date:** YYYY-MM-DD
**Trigger:** One-line description of what prompted the most recent update.

## Update Count
1

## Open Questions
- Things Claude has flagged as uncertain, contested, or requiring user input.
- Remove a question when resolved; note the resolution in the Revision Log.
- This section can be left empty if there are no open questions.

## Human Notes
> This section is yours. Add comments, corrections, context, or anything else here.
> Claude reads this section but will never overwrite it.
> You can use plain prose, bullet points, or whatever format suits you.

---

## Revision Log
*(newest at top)*

### YYYY-MM-DD — [brief title of change]
What changed and why. 2–4 sentences is enough. Be specific about what shifted
and what prompted the shift — a decision, a discovery, a user correction, etc.
```

---

## Section Reference

| Section | Required | Who writes it | Notes |
|---|---|---|---|
| `## Current State` | Yes | Claude (with approval) | Replaced on each update, not appended |
| `## Confidence` | Yes | Claude (with approval) | High / Medium / Low + reason |
| `## Last Updated` | Yes | Claude (with approval) | Date + one-line trigger |
| `## Update Count` | Yes | Claude (with approval) | Integer, increments each update |
| `## Open Questions` | Optional | Claude (with approval) | Can be empty |
| `## Human Notes` | Optional | User only | Claude never writes here |
| `## Revision Log` | Yes | Claude (with approval) | Append-only, newest at top |

---

## Naming Convention

Name entry files in lowercase with hyphens: `target-persona.md`, `pricing-model.md`, `core-feature-set.md`.

The index file is always named `_index.md` and lives at the root of your wiki folder.

---

## Index File Structure

`_index.md` is the map Claude reads first at the start of every session. Keep it scannable.

```markdown
# Wiki Index

## All Entries

| Entry | Summary |
|---|---|
| [Entry Title](filename.md) | One-line summary of current understanding |

## Hot Topics
Entries with the highest update counts or most recent changes. Claude prioritises these.

- **[Entry Title]** (Update Count: N) — reason it's hot
- **[Entry Title]** (Update Count: N) — reason it's hot

## Last Session
**Date:** YYYY-MM-DD
Brief note on what was being worked on, so Claude can orient quickly at the start of the next session.
```

---

## Design Notes

**Why plain headers, not YAML frontmatter?**
YAML frontmatter requires precise formatting — a missing colon or extra space can break a parser. Plain `##` headers are tolerant of human edits. You can add a sentence, reformat a bullet, or move a section and Claude will still find what it needs.

**Why append-only revision log?**
The log is an audit trail, not a summary. Overwriting past entries would destroy the record of how understanding evolved. The newest entry is always at the top so it's easy to read; the full history is preserved below for reference.

**Why no YAML, JSON, or special syntax in entries?**
The schema is designed to be readable and editable by anyone in a plain text editor. No tooling required. The wiki should work even if Claude Code is unavailable — a human should be able to read and edit every file without documentation.
