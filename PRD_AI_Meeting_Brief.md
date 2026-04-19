# PRD: AI Meeting Brief

**Product:** Workstream (B2B SaaS Project Management)  
**Author:** Madhumathi  
**Status:** Draft  
**Date:** June 2025  
**Stakeholders:** Product, Engineering (2 FE, 1 BE, 1 ML), Design, Customer Success

---

## Description

What is it?

A feature that automatically generates a one-page project brief 30 minutes before every calendar meeting linked to a Workstream project. The brief surfaces open blockers, tasks due soon, recent decisions, and the latest comments -- so users walk into meetings prepared without spending 10-15 minutes manually pulling context from Slack, email, and their project board.

---

## Problem

What problem is this solving?

There is no connection between a user's calendar and their project context in Workstream. Before any project meeting, users manually reconstruct what is happening across multiple tools. This context-switching costs time, causes people to show up underprepared, and trains users to treat Workstream as a secondary tool rather than a source of truth.

---

## Why

How do we know this is a real problem and worth solving?

- User interviews (n=14, May 2024): 11 of 14 said they manually piece together context before project meetings from Slack, email, and Workstream separately.
- Support ticket analysis: the tag "meeting-prep" appeared 63 times in Q1 2024. Users were asking how to export a project status snapshot.
- NPS detractor verbatim: "I love Workstream but I still open 4 tabs before every standup."
- Competitor signal: Notion AI launched a meeting prep feature in March 2024. In a follow-up survey of shared users, 38% cited it as their most-used AI feature.
- Retention data: Users who access Workstream on meeting days have 22% higher DAU than users who do not, suggesting that meeting context is a natural re-engagement trigger we are not yet capturing.

---

## Success

How do we know if we have solved this problem?

| Metric | Baseline | Target | Timeframe |
|--------|----------|--------|-----------|
| Self-reported meeting prep time (in-app survey) | ~12 min | Under 4 min | 90 days post-launch |
| DAU gap between meeting days and non-meeting days | 22% gap | Gap closed to under 10% | 60 days post-launch |
| Weekly active teams using Meeting Brief | 0 | 35% of eligible teams | 90 days post-launch |
| 90-day retention: brief users vs. control | Baseline TBD in beta | +8 percentage points | 90 days post-launch |

Kill criteria: if brief open rate is below 20% at 60 days, or LLM cost per brief exceeds $0.08 without a path to compression, we pause and reassess.

---

## Audience

Who are we building for?

Primary user: Maya, Senior PM at a 120-person SaaS company. Runs 3 project standups per week, manages 6 active workstreams, frequently moves between meetings with no buffer time. Her goal is to stay on top of project health without manual overhead.

Secondary user: Team leads and ICs who receive the shared brief before a meeting and use it to prepare their own updates.

Not in scope for v1: external stakeholders who do not use Workstream, users whose meetings are not linked to a project.

---

## Non-goals

What are we explicitly not building in this release?

- Meeting notes or transcription (separate roadmap item)
- Video call integration with Zoom or Google Meet
- Post-meeting action item generation
- Briefs for meetings with no linked Workstream project

---

## What

What does this look like in the product?

**Brief structure (v1)**

```
MEETING BRIEF -- [Project Name]
[Meeting title] | [Date] | [Time]

OPEN BLOCKERS (2)
  API rate limit issue blocking QA -- Ravi -- 3 days open
  Design sign-off pending on checkout flow -- no owner

DUE IN 48 HOURS (3)
  Finalize onboarding email copy -- Maya -- due today
  QA sign-off on v2.1 -- Dev team -- due tomorrow
  Stakeholder deck -- Priya -- due tomorrow

DECISIONS SINCE LAST MEETING
  Agreed to delay feature flag rollout to avoid Q3 freeze
  Switched payment processor to Stripe (thread linked)

LAST ACTIVITY
  Ravi: "Pushed a fix for the API issue, needs review"
  Priya: "Deck is 80% done, sending EOD"
```

**How it works**

- Calendar integration via Google Calendar and Outlook OAuth (already in auth scope). Reads meeting title, time, and attendees.
- User links a project to a recurring meeting series one time. Stored as a meeting-series-to-project mapping.
- At T-30 minutes, a backend job queries project activity since the last brief timestamp and passes structured data to an LLM (GPT-4o or Claude Sonnet) with a strict output schema. The LLM summarizes and prioritizes -- it does not generate facts.
- Output is stored as a brief object linked to project ID and meeting ID, then rendered client-side from JSON.
- Users can also generate a brief on demand up to 3 times per project per day.
- One-click copy as plain text. Optional shareable link, view-only, expires after 24 hours.

**Edge cases**

| Scenario | Handling |
|----------|----------|
| No project activity since last brief | Show "No new updates" plus last known state |
| No linked project | Prompt to link; no brief generated until linked |
| Project has 500+ tasks | Surface only tasks tagged high-priority or assigned to meeting attendees |
| User is not a project member | Show only public tasks; no private comments |
| LLM call fails | Fallback to structured task and blocker list without summary layer |

---

## Launch plan

**Phase 1 -- Closed beta (Weeks 1-4)**  
5 existing customer teams sourced by CS. Manual project linking only. Collect: time-to-open, brief quality rating (thumbs up/down), qualitative feedback.

**Phase 2 -- Expanded beta (Weeks 5-8)**  
50 teams across SMB and mid-market. Auto-generation enabled. Instrument: brief open rate, share rate, DAU on meeting days vs. control.

**Phase 3 -- GA (Week 9+)**  
Available to all Team and Business plan customers. In-app education via tooltip and one email touchpoint. CS enablement with an upsell talk track (Meeting Brief is a Team plan feature).

---

## Risks

| Risk | Likelihood | Impact | Mitigation |
|------|-----------|--------|------------|
| LLM output is inaccurate | Medium | High | Strict schema -- LLM summarizes structured data, not freeform. Source links on every item. |
| High setup friction kills adoption | High | High | Project-linking flow must complete in under 60 seconds. Pre-populate suggestions by matching meeting title to project names. |
| Cost per brief too high at scale | Medium | Medium | Cache briefs for 30 minutes. Rate-limit manual generation. Monitor cost per brief in week 1 of beta. |
| Calendar permission request reduces signups | Low | Medium | Request permission at first relevant moment, not during onboarding. Explain exactly what is accessed. |

---

## Open questions

1. Should brief generation be opt-in or opt-out by default? Opt-in reduces adoption risk. Opt-out drives higher initial usage but may feel intrusive.
2. Should non-Workstream users be able to view a shared brief link without creating an account? Potential growth loop but adds auth complexity.
3. If a project is linked to multiple recurring meeting series, should each series get an independent brief or a shared one updated per meeting?
4. What is the right thumbs-up/thumbs-down threshold before triggering a re-generation vs. a flag for manual review?

---

## Appendix

- [User interview recordings -- Notion]
- [Competitive analysis -- Figma]
- [Design mockups -- Figma]
- [Technical spec -- Confluence]
