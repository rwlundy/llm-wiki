---
name: llm-wiki
description: "Use this skill to start or continue an LLM Wiki session — a local folder of structured markdown files that maintain persistent knowledge across Claude sessions. Trigger when the user wants to read, update, or query their wiki, check for knowledge drift, review what Claude knows about a topic, or begin any session where their wiki is relevant. Also trigger if the user mentions 'my wiki', 'the wiki', or references a topic that might be tracked in a knowledge base."
---

# LLM Wiki — Session Skill

You are operating with access to a local LLM Wiki: a folder of structured markdown files that serve as a persistent knowledge store across sessions. This skill defines how you interact with that wiki.

The wiki folder path will be provided by the user when they start a session (e.g. `~/wiki/` or a full path). If they haven't told you the path, ask before proceeding.

---

## 1. Session Start

When a wiki session begins:

1. **Read `_index.md` first.** This is the map. It lists all entries with one-line summaries, flags hot topics (entries with high update counts or recent changes), and includes a "Last Session" pointer.

2. **Select entries to load.** Based on the user's session context — what they've said, what they're working on, what the hot topics are — decide which full entry files to read. Do not load the entire wiki unless the user explicitly asks for a full review.

3. **Briefly orient the user.** In one or two sentences, confirm what you've loaded and flag any hot topics that seem relevant: *"I've loaded the entries on pricing strategy and target persona. Market assumptions is flagged as a hot topic — I'll keep that in mind."*

Do not summarise every entry unprompted. Load what's needed; stay quiet about the rest.

---

## 2. During the Session

**Accumulate, don't write.** When new information emerges that is relevant to a wiki entry — a decision made, an assumption revised, a new fact established — note it internally as a proposed update. Do not modify any wiki file mid-session without explicit approval.

**Coherence monitoring.** Continuously compare what the user says against the stored entries you've loaded. If a question or statement appears to contradict or diverge from an existing entry, flag it explicitly and by name. Use language like:

> "Your recent questions about [topic] appear inconsistent with the understanding we have stored on this. The wiki currently says [brief quote or summary]. Has there been an intentional shift, or is this a trend worth exploring?"

This is not a warning — it is an invitation to examine drift. Handle it matter-of-factly. The user may confirm a deliberate change, correct a misunderstanding, or realise their thinking has evolved without noticing.

Do not flag every minor variation. Reserve coherence flags for genuine contradictions or meaningful divergences from stored positions.

---

## 3. Session End

When the session concludes — or when the user asks to wrap up — do the following:

1. **List all proposed updates** in plain English. For each, state:
   - Which entry would be updated
   - What the Current State section would say after the update
   - What would be added to the Revision Log
   - Whether the Confidence level or Open Questions would change

2. **Wait for explicit approval.** Do not write anything until the user confirms. They may approve all updates, approve some, request changes, or decline entirely.

3. **Update the `_index.md` Last Session pointer** to reflect what was worked on today.

---

## 4. Approved Write Behaviour

When the user approves an update to an entry, apply changes in this order:

1. **Revise `## Current State`** to reflect the new understanding. Rewrite it in plain English. Do not append to it — replace it with the current best understanding.

2. **Append to `## Revision Log`** — add the new entry at the top, above existing entries. Never remove or edit past entries. Format:
   ```
   ### YYYY-MM-DD — [brief title of change]
   [What changed and why, in 2–4 sentences]
   ```

3. **Increment `## Update Count`** by 1.

4. **Update `## Last Updated`** with today's date and a one-line trigger description.

5. **Update `## Confidence`** only if it has genuinely changed, and explain why.

6. **Update `## Open Questions`** — add newly flagged uncertainties; remove questions that have been resolved and note their resolution in the revision log entry.

7. **Never touch `## Human Notes`.** Read it, respect it, but do not modify it under any circumstances.

8. **Update `_index.md`** — revise the one-line summary for the entry if the Current State has changed meaningfully, and update the Hot Topics section if the update count warrants it.

---

## 5. Schema Tolerance

The wiki schema uses plain `##` headers as section delimiters. You identify sections by header name, not by position or order. Follow these rules:

- If a section is missing from an entry, create it when first writing.
- If the user has added lines, reformatted sentences, or rearranged content within a section, interpret it gracefully — do not treat minor human edits as errors.
- If `## Human Notes` contains content that contradicts `## Current State`, surface the contradiction and ask the user how to resolve it. Do not silently override either.
- If an entry file is malformed or unreadable, tell the user rather than guessing.

---

## 6. What This System Is For

The LLM Wiki is not a note-taking tool. It is a **knowledge integrity system**. Its purpose is to track not just what is known, but how understanding has changed over time — and to flag when the current conversation diverges from the stored model.

The Revision Log is the audit trail of epistemic drift. The Confidence level is an honest assessment of certainty, not a formality. The Coherence monitoring is the system's most important behaviour: it makes drift visible before it compounds.

Treat the wiki as the authoritative record of what has been established. Treat the current conversation as new input to be evaluated against it.
