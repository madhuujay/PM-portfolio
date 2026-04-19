# PRD: Pulse -- Burnout Early Warning System

**Product:** Finch (B2B SaaS HR Platform)  
**Author:** [Your Name]  
**Status:** Draft  
**Date:** June 2024  
**Stakeholders:** Product, Engineering (2 FE, 1 BE), Design, People Ops, Legal

---

## Description

What is it?

A passive, privacy-first signal layer built into Finch that detects early burnout patterns across a team -- without surveying employees directly. Pulse reads behavioral signals that already exist inside the tools a company uses (calendar density, after-hours activity, PTO non-usage, meeting-to-focus-time ratio) and surfaces a weekly heatmap to managers with nudges on who may need a check-in. No scores are shown to employees. No data is shared upward beyond the direct manager.

---

## Problem

What problem is this solving?

Managers find out an employee is burned out when they resign or go on leave. By that point the cost to the company is already realized: lost productivity, a backfill hire at 50-200% of annual salary, and institutional knowledge that walks out the door. HR platforms today offer pulse surveys, but surveys have two fatal flaws: burned-out employees either do not fill them out or fill them out dishonestly to avoid stigma. The signal arrives too late or not at all.

---

## Why

How do we know this is a real problem and worth solving?

- Gallup's 2023 State of the Global Workplace report found that 44% of employees experienced significant stress the previous day -- the highest level recorded in Gallup's history.
- Microsoft's 2023 Work Trend Index found that after-hours and weekend work increased 28% since 2020 and has not declined.
- User interviews with HR leaders (n=11, April 2024): 9 of 11 said they learned about burnout cases "too late to intervene." The other 2 said they only caught it because the employee's manager happened to notice.
- Finch customer support data: "burnout" and "attrition" appeared in 41 account health calls in Q1 2024. Customers are asking for this, just not knowing what to ask for.
- Internal analysis of churned Finch accounts (n=28, past 18 months): 17 cited "couldn't prove ROI of people investment" as a reason for leaving. Pulse directly addresses this gap by tying manager action to measurable retention outcomes.
- Survey fatigue is real and well-documented: average pulse survey response rates across HR SaaS platforms sit between 30-55% according to Qualtrics and Culture Amp benchmark data. The employees least likely to respond are the ones most at risk.

---

## Success

How do we know if we have solved this problem?

| Metric | Baseline | Target | Timeframe |
|--------|----------|--------|-----------|
| Manager check-in rate after Pulse nudge | Unknown (establishing baseline in beta) | 60% of nudges result in a logged check-in within 5 days | 90 days post-launch |
| Voluntary attrition in Pulse-enabled teams vs. control | Baseline from Finch HR data | 15% reduction in voluntary attrition at 6 months | 6 months post-launch |
| Feature adoption: managers who open Pulse weekly | 0 | 40% of eligible managers weekly | 60 days post-launch |
| Expansion revenue from Pulse upsell | 0 | $180K ARR from add-on pricing in first 2 quarters | 6 months post-launch |

Kill criteria: if manager check-in rate is below 25% at 60 days, the nudge mechanism is not working and we reassess the intervention design before continuing rollout.

---

## Audience

Who are we building for?

Primary user (consumer of insights): Jordan, an Engineering Manager at a 200-person SaaS company. Manages a team of 7. Has back-to-back meetings most days and relies on gut feel to know when someone is struggling. Wants to be a good manager but does not have time to proactively monitor everyone. Would act on a well-timed signal if it landed in front of them.

Secondary user (HR admin): The People Ops lead who configures which signals Pulse is allowed to read, sets the privacy policy visible to employees, and reviews aggregate team-level trends (not individual data).

Not the user: The employee. Employees are never shown their own Pulse signal, score, or status. They see only a company-wide notice that Finch Pulse is enabled and a plain-language explanation of what data is and is not collected.

---

## Non-goals

What are we explicitly not building in this release?

- An employee-facing wellness dashboard or score
- Integration with personal health data, wearables, or any data outside work tools
- Upward reporting -- no CEO or CHRO view of individual employee signals
- Performance management linkage -- Pulse data is never surfaced in review cycles
- Automated interventions -- Pulse nudges managers, it does not send anything to employees directly
- A surveillance tool -- if a use case requires tracking individual productivity, that is a different product

---

## What

What does this look like in the product?

**Signals Pulse reads (configurable by HR admin)**

| Signal | What it measures | Why it matters |
|--------|-----------------|----------------|
| Calendar density | Meetings as a percentage of work hours | High meeting load with no focus time is a leading burnout indicator |
| After-hours activity | Slack/email activity outside 8am-7pm local time | Consistent late-night activity predicts exhaustion within 4-6 weeks |
| PTO non-usage | Days accrued vs. days taken in rolling 90 days | Employees who stop taking time off are often disengaging or afraid to disconnect |
| Meeting-to-focus ratio | Ratio of scheduled meetings to uninterrupted blocks over 60 min | Below 1:1 ratio correlates with reported creative and cognitive fatigue |
| Response latency trend | Change in average Slack response time over 30 days | A sudden increase in response latency can signal withdrawal |

HR admin can turn any signal on or off. Default configuration ships with calendar density, after-hours activity, and PTO non-usage only -- the least sensitive set.

**Manager view: weekly Pulse heatmap**

A single card in the manager's Finch homepage. Shows their direct reports as a row of colored indicators: green (no flags), amber (one signal elevated), red (two or more signals elevated for 2+ consecutive weeks). No names attached to red or amber status in any shared or exportable view. Clicking a name opens a private detail view visible only to that manager.

**The nudge**

When a direct report has been in amber or red for 10 or more days, Finch sends the manager a nudge via email and in-app notification. The nudge does not name a diagnosis. It says:

"One of your direct reports may be showing signs of overload. This is a good week for a one-on-one check-in focused on workload and capacity rather than project status. Here are three questions to open the conversation."

The three questions are generated based on which signals are elevated (e.g. if after-hours activity is high, one question is about boundaries and disconnecting).

**Check-in logging**

After a nudge, the manager can log that they had a check-in with a single click and an optional free-text note (visible only to them). This closes the loop and resets the nudge timer. It also feeds the success metric above.

**Employee transparency notice**

Every employee at a Pulse-enabled company sees a plain-language notice in their Finch profile explaining: what signals are collected, that individual data is visible only to their direct manager, that no data flows into performance reviews, and how to contact People Ops with concerns. This notice is not opt-out -- it is informational. The opt-out decision sits at the company level, not the individual level, which Legal has reviewed and approved for applicable jurisdictions (see open questions for exceptions).

**Edge cases**

| Scenario | Handling |
|----------|----------|
| Manager is the one showing burnout signals | Their signals are visible to their manager one level up, same rules apply |
| Employee is on a performance improvement plan | Pulse data is explicitly excluded from PIP documentation by a system lock |
| Employee is in a jurisdiction with stricter monitoring laws (Germany, Netherlands) | Pulse is disabled by default for those geographies; HR admin must explicitly enable with additional consent flow |
| Employee leaves the company | Their signal history is deleted within 30 days of offboarding |
| Manager ignores nudges repeatedly | After 3 unacknowledged nudges, People Ops admin receives an aggregate flag (not individual data) that a team may need support |

---

## Launch plan

**Phase 1 -- Design partner beta (Weeks 1-6)**  
3 Finch enterprise customers who have flagged retention as a top concern. HR admin configures signals. Collect: manager open rate on heatmap, check-in logging rate, qualitative feedback from People Ops leads. No nudges in this phase -- observation only.

**Phase 2 -- Nudge beta (Weeks 7-12)**  
Expand to 15 accounts. Enable nudges. A/B test nudge copy: directive ("Schedule a check-in this week") vs. supportive ("Here is a good moment to connect on workload"). Measure check-in rate and manager sentiment via a 2-question in-app survey.

**Phase 3 -- GA with add-on pricing (Week 13+)**  
Available as a paid add-on to Finch Team and Enterprise plans. Price: $4 per employee per month. Legal review complete for US, UK, Canada, Australia. EU rollout requires additional consent architecture (scoped to Q3).

---

## Risks

| Risk | Likelihood | Impact | Mitigation |
|------|-----------|--------|------------|
| Employees feel surveilled and trust erodes | High | Very high | Radical transparency notice. No employee-facing scores ever. Legal review per jurisdiction. Privacy by design -- signals are aggregated, not logged as raw activity. |
| Managers misuse signals as performance data | Medium | Very high | System lock preventing Pulse data from appearing in any review workflow. Training module required before manager access is activated. |
| Signal accuracy is low -- too many false positives | Medium | High | Ship with conservative defaults (only 3 signals). Let HR admin tune thresholds. Nudge only fires after 10+ days of elevated signals, not a single spike. |
| Legal exposure in EU / Germany | High (in those markets) | High | Default-off in restricted jurisdictions. Works-council approval flow built into onboarding for EU accounts. Separate legal review before EU GA. |
| Low manager engagement -- heatmap ignored | Medium | Medium | Embed heatmap in the existing manager homepage, not a separate tab. Make the default state require zero effort -- it is ambient, not a new task. |

---

## Open questions

1. Should employees be able to see their own signal trend, even in aggregate form, as a personal wellness tool? Argument for: gives employees agency. Argument against: creates anxiety and second-guessing of their own behavior.
2. For the EU rollout, what is the minimum viable consent architecture that satisfies GDPR Article 88 (employment context) without making the feature unusable?
3. Should Pulse data be retained after an employee's manager changes? Current proposal is to reset the signal baseline on manager transition to avoid a new manager inheriting context that could bias them.
4. What happens if a manager uses the check-in log note field to write something that creates legal liability (e.g. documenting a medical disclosure)? Do we need a content warning or character restriction?
5. Is $4 per employee per month the right price, or does this need to be bundled into the top-tier plan to drive adoption among the accounts where retention risk is highest?

---

## Appendix

- [User interview recordings -- Notion]
- [Legal memo: monitoring laws by jurisdiction -- Confluence]
- [Design mockups: manager heatmap and nudge flow -- Figma]
- [Signal accuracy analysis: methodology and assumptions -- Notion]
- [Competitive landscape: Leapsome, Lattice, Culture Amp -- Figma]
