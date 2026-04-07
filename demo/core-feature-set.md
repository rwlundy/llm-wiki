# Core Feature Set

## Current State
Meridian's MVP feature set is:

1. **Async check-in prompts** — a team admin schedules a recurring prompt (e.g. "What's your focus this week? Any blockers?"). Team members respond in their own time with a short video or text response (90-second cap on video).

2. **Response feed** — a shared, chronological feed of responses for each prompt cycle. Visible to the whole team by default. No commenting, no reactions in v1.

3. **Digest notifications** — a daily or weekly summary notification that tells team members when responses are in, without requiring them to log in to check.

4. **Admin dashboard** — prompt scheduling, team member management, participation tracking (who has and hasn't responded).

Third-party integrations (Slack, Notion, Linear) are **explicitly out of scope for MVP**. They were briefly scoped in during Q1 but removed in March 2026 following a scope review. The integration surface adds maintenance burden and delays the core loop. We will revisit post-launch when we have real usage data.

The 90-second video cap is a deliberate constraint, not a technical limitation. It forces brevity and makes the check-in feel low-stakes.

## Confidence
**Level:** High
**Reason:** MVP scope was locked in March 2026 after removing integrations. The remaining feature set is stable and agreed by the team.

## Last Updated
**Date:** 2026-03-28
**Trigger:** Integrations removed from MVP scope following scope review meeting on 2026-03-25.

## Update Count
4

## Open Questions
- Should responses be visible to the whole team by default, or should we default to manager-only visibility and let teams opt into open sharing? Privacy implications are unresolved.
- Is a 90-second video cap the right constraint, or should we pilot 60s and 120s with early users?

## Human Notes
> Removing integrations felt like the right call but it was a contentious discussion. The Slack integration especially — some people feel like without it, we're asking users to form a new habit with no bridge from where they already are. Worth revisiting quickly post-launch.

---

## Revision Log
*(newest at top)*

### 2026-03-28 — Slack and Notion integrations removed from MVP scope
Scope review on 2026-03-25. Integrations were adding 6–8 weeks of estimated build time and significant ongoing maintenance risk. Decision: ship the core async loop first, validate retention and engagement, then add integrations based on real usage. Slack integration deferred to v1.1; Notion integration moved to backlog pending demand signal.

### 2026-02-10 — Linear integration added to scope, Notion integration scoped
Following two customer conversations where users asked how Meridian would connect to their project management workflow, Linear and Notion integrations were added to MVP scope. Estimated as a 4-week addition. Team agreed to include them.

### 2025-12-01 — Structured check-in prompts added; Slack integration scoped
Original concept was free-form async video only (users record whatever they want). User research showed teams wanted structure — a shared prompt so responses are comparable and easier to scan. Prompt scheduling and the response feed added to core feature set. Slack integration raised as a potential distribution channel; added to MVP scope as a stretch goal.

### 2025-10-15 — Initial feature set defined
MVP defined as: async video recording (free-form), shared team feed, basic admin controls. No integrations. No structured prompts. Core hypothesis: teams will adopt async video if the recording experience is fast enough.
