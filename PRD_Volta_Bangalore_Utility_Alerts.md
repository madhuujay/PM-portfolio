# PRD: Volta -- Bangalore Utility Outage Intelligence App

**Product:** Volta (Consumer Mobile App, Android and iOS)  
**Author:** [Your Name]  
**Status:** Draft  
**Date:** June 2024  
**Stakeholders:** Product, Engineering (2 Mobile, 1 BE, 1 Data), Design, Growth

---

## Description

What is it?

A hyperlocal mobile app for Bangalore residents that gives them advance warning of BESCOM power cuts and BWSSB water supply disruptions -- before they happen, not after. Volta aggregates BESCOM's scheduled outage PDFs, BWSSB maintenance advisories, and crowd-sourced reports from residents to deliver a single, address-specific alert feed. Users enter their layout or apartment name once and never have to check a government website, hunt through a PDF, or wait until their tap runs dry or their laptop dies.

---

## Problem

What problem is this solving?

Bangalore residents find out about power cuts when their lights go off and about water disruptions when their taps run dry. BESCOM publishes scheduled outage notices -- but they are posted as scanned PDFs on a government website, organized by division name (not by area or apartment), updated inconsistently, and written in a format that requires you to know your feeder number. BWSSB issues maintenance advisories for events like the 60-hour Cauvery shutdown in September 2024 -- but residents only found out through news articles and WhatsApp forwards, hours or days late.

The result is not just inconvenience. It is a cascade of downstream problems: laptops dying mid-call for remote workers, inverters and generators not charged, water tanks not filled, food spoiled in refrigerators, medicines requiring refrigeration compromised, and washing machine cycles interrupted with no water to complete them. For a city with 13 million people, most of them working in tech jobs from home or small offices, this is a solved problem waiting for someone to build it.

---

## Why

How do we know this is a real problem and worth solving?

- BESCOM publishes planned outage schedules that can run up to 9 hours per day, but the information is buried in division-level PDFs on bescom.karnataka.gov.in that most residents have never opened.
- BWSSB shut down the entire Cauvery water supply for 60 hours in September 2024 for maintenance across Stages I through V. Residents were advised to store water -- but the advisory reached most people through WhatsApp forwards and news articles, not any direct notification channel.
- BWSSB supplies around 1,460 million litres per day to Bangalore but still meets only about 50% of the city's total water demand. Supply is alternate-day in many areas and once-a-week in outer layouts. Residents have no reliable way to know when their slot is.
- BESCOM's app, BESCOM Mithra, exists but requires users to know their RR number (consumer number) to register. Most tenants, PG residents, and new apartment dwellers do not have this. The app has 4,000 reviews on the Play Store with an average of 2.8 stars, dominated by complaints about crashes and missing notifications.
- r/bangalore has recurring monthly threads titled "how do I know when BESCOM will cut power in my area" with hundreds of upvotes. The top answer is always a workaround -- follow BESCOM on Twitter and search your area name, check a third-party news site, ask your apartment WhatsApp group.
- User interviews with Bangalore residents (n=16, May 2024): 14 of 16 said they learned about their last power cut after it had already started. 12 of 16 said they had been caught with an uncharged laptop or phone during a cut in the past month. 9 of 16 said they had run out of stored water during a BWSSB disruption.

---

## Success

How do we know if we have solved this problem?

| Metric | Baseline | Target | Timeframe |
|--------|----------|--------|-----------|
| Alert delivered before outage starts (advance notice rate) | 0% (no product exists) | 80% of scheduled outages alerted 12+ hours in advance | 60 days post-launch |
| D30 retention | 0 | 45% | 60 days post-launch |
| Weekly active users | 0 | 50,000 WAU | 90 days post-launch |
| Crowd-report submissions per day | 0 | 500 per day by month 2 | 60 days post-launch |
| App store rating | N/A | 4.2 or above | 90 days post-launch |

Kill criteria: if D30 retention is below 20% at 60 days, the alert relevance or timing is off and we pause growth spend to fix the core loop before scaling.

---

## Audience

Who are we building for?

Primary user: Priya, 28, software engineer, rents a 1BHK in HSR Layout. Works from home 4 days a week. Relies on her laptop, router, and phone staying charged. Has a water tank in the building but has no idea when BWSSB supplies her area. Has been burned multiple times -- once lost 3 hours of work to an unannounced 6-hour cut, once ran out of water for two days because the tank was not filled before a maintenance shutdown. Checks her apartment WhatsApp group 10 times a day to find out this information.

Secondary user: Apartment association secretary or building manager. Wants to forward utility alerts to residents without doing manual research every day. Would use Volta as the source of truth for their building's WhatsApp broadcast.

Also building for: PG residents, small office owners, families with elderly members or infants (water dependency is acute), and home-based business owners (salon, tutor, home baker) for whom a 6-hour power cut is a direct revenue loss.

Not the primary user in v1: areas outside BBMP limits (BESCOM and BWSSB data coverage is weakest in outer layouts and new extensions -- we will expand coverage as data pipelines mature).

---

## Non-goals

What are we explicitly not building in this release?

- Bill payment for BESCOM or BWSSB (a separate workflow, a separate intent)
- Complaint filing with BESCOM or BWSSB (useful but not the core problem)
- Water tanker booking
- Solar or inverter management
- Coverage outside Bangalore in v1

---

## What

What does this look like in the product?

**Onboarding (under 60 seconds)**

User enters their area name or apartment name -- not their consumer number, not their RR number. Volta fuzzy-matches it to a BESCOM division and BWSSB zone. User confirms the match. Done. No login required to receive alerts in v1.

**Home screen**

Two cards, always visible:

Card 1 -- Power  
"No cuts scheduled in HSR Layout today."  
or  
"Power cut tomorrow, 10am to 4pm. BESCOM maintenance. 18 hours from now."

Card 2 -- Water  
"Cauvery supply expected today between 6am and 9am."  
or  
"No supply tomorrow. BWSSB maintenance. Fill your tank tonight."

Tapping either card opens a detail view: reason for the outage, affected streets if known, BESCOM division or BWSSB zone, a crowd-report thread for that outage, and a "Set reminder" option.

**Alerts**

Push notification sent at three points: 24 hours before, 2 hours before, and when the outage starts. User can configure which of these they want. Default is 2 hours before only (lowest friction, highest utility).

**Crowd reports**

Any user can tap "Report an issue" to submit: unannounced power cut, water not arrived at expected time, supply restored earlier than stated. Reports are aggregated -- if 5 or more users in the same area submit the same type of report within 30 minutes, Volta surfaces a community alert for that area: "Residents in HSR Layout Sector 2 reporting an unannounced power cut right now."

Crowd reports require no account. Users tap once. The submission is tied to their approximate location (layout level, not street level).

**Data sources and pipeline**

| Source | What it provides | How we ingest it |
|--------|-----------------|-----------------|
| BESCOM planned outage PDFs | Scheduled division-level cuts with date and time | Daily scraper, PDF parser, mapped to layout names |
| BESCOM social media (X/Twitter) | Real-time fault updates and restoration ETAs | API pull, keyword filter by area name |
| BWSSB press releases and maintenance advisories | Scheduled shutdowns and supply timing | Daily scraper from bwssb.karnataka.gov.in |
| BWSSB supply schedule (static) | Alternate-day or weekly schedule by zone | One-time import, updated manually on changes |
| Crowd reports (Volta users) | Unannounced cuts, early restorations, actual vs. scheduled | In-app submission, aggregated before surfacing |

**Edge cases**

| Scenario | Handling |
|----------|----------|
| User's area not matched to a BESCOM division | Show "We are still mapping your area" with a manual entry fallback and a waitlist for that zone |
| BESCOM PDF format changes (happens regularly) | Parser health check runs daily; alert to engineering if parse rate drops below 80% |
| Unannounced cuts (no BESCOM notice) | Crowd reports fill the gap; Volta surfaces a community alert if threshold is met |
| User moves to a new area | Re-onboarding prompt if location changes significantly; option to add a second location (for office) |
| BWSSB supply runs late | No penalty -- Volta shows expected time from schedule and crowd reports update actual arrival |
| False crowd reports | Reports are aggregated, not individual. A single troll report does not surface. Threshold is 5+ reports in 30 minutes. |

---

## Monetisation (v1 thinking, not in scope for this release)

Three directions to evaluate post-PMF:

1. Freemium: free for one location, paid (Rs. 49/month) for multiple locations and SMS alerts (useful for people managing a home and an office or an elderly parent's house).
2. Building manager tier: Rs. 299/month for an account that covers an entire apartment complex and allows broadcast to residents.
3. B2B: sell aggregated, anonymised outage pattern data to solar panel companies, inverter manufacturers, and water storage brands running hyperlocal campaigns.

---

## Launch plan

**Phase 1 -- Closed beta (Weeks 1-4)**  
200 users across 5 layouts: HSR Layout, Koramangala, Indiranagar, Jayanagar, Whitefield. Manual data pipeline (team curates BESCOM PDFs daily). Measure: alert advance notice rate, D7 retention, crowd report submission rate. No growth spend.

**Phase 2 -- Automated pipeline + open beta (Weeks 5-10)**  
Automated BESCOM scraper and BWSSB parser live. Open waitlist signups. Add 20 more layouts based on user demand signals from Phase 1. Measure: D30 retention, alert accuracy rate (scheduled vs. actual outage match).

**Phase 3 -- Public launch (Week 11+)**  
Play Store and App Store listing. PR through r/bangalore, apartment association networks, and tech Twitter. Target: 50,000 WAU by end of month 3.

---

## Risks

| Risk | Likelihood | Impact | Mitigation |
|------|-----------|--------|------------|
| BESCOM PDF format changes break the parser | High | High | Daily parser health checks. Fallback to BESCOM Twitter scraper. Manual override by ops team if parser fails. |
| Low D30 retention if alerts are inaccurate | High | Very high | Phase 1 is specifically designed to measure and fix this before any growth spend. Accuracy is the only metric that matters in beta. |
| BWSSB data is inconsistent or unavailable | Medium | Medium | Crowd reports compensate. Show "schedule not available" rather than a wrong alert. |
| Crowd reports abused or spammed | Low | Medium | Aggregation threshold of 5+ reports prevents individual abuse. Location required for submission. |
| BESCOM or BWSSB sends a legal notice for scraping | Low | High | Data is public. We display source attribution on every alert. Legal review before launch on scraping terms. |

---

## Open questions

1. Should Volta require an account at all in v1, or is a fully anonymous experience (location-based, no login) better for early retention?
2. How do we handle areas where BESCOM data is genuinely unavailable -- do we show "no data" or hide those areas from onboarding entirely to avoid false confidence?
3. Is the 5-report threshold for crowd alerts too high for small layouts where total users might be under 20? Should the threshold be percentage-based rather than absolute?
4. Should the building manager tier be built before or after consumer PMF? There is a case for building it early since apartment secretaries are high-leverage distribution.
5. Should alerts be sent via WhatsApp in addition to push notifications? WhatsApp has higher open rates in India but adds complexity and cost.

---

## Appendix

- [User interview notes -- Notion]
- [BESCOM PDF scraper prototype -- GitHub]
- [BWSSB zone mapping spreadsheet -- Google Sheets]
- [Competitor review: BESCOM Mithra Play Store analysis -- Notion]
- [Design mockups -- Figma]
