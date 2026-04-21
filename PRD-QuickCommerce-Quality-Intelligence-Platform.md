# PRD: Quality Intelligence Platform for Quick Commerce
**Document Type:** Product Requirements Document  
**Author:** Madhumathi 
**Last Updated:** April 2026  
**Product Area:** Customer Experience & Operations Quality

---

## Table of Contents

1. [Executive Summary](#1-executive-summary)
2. [Problem Statement](#2-problem-statement)
3. [Goals & Success Metrics](#3-goals--success-metrics)
4. [User Personas](#4-user-personas)
5. [Market Context](#5-market-context)
6. [Solution Overview](#6-solution-overview)
7. [Feature Requirements](#7-feature-requirements)
8. [Query Pattern Intelligence Engine](#8-query-pattern-intelligence-engine)
9. [Quality Control Framework](#9-quality-control-framework)
10. [User Stories](#10-user-stories)
11. [Non-Functional Requirements](#11-non-functional-requirements)
12. [Out of Scope](#12-out-of-scope)
13. [Dependencies & Risks](#13-dependencies--risks)
14. [Timeline & Milestones](#14-timeline--milestones)
15. [Open Questions](#15-open-questions)

---

## 1. Executive Summary

Quick commerce platforms promise delivery in 10–30 minutes, but this speed often comes at the cost of product quality — damaged goods, wrong items, near-expiry products, and poor packaging. This leads to a high volume of customer support queries, refund requests, and churn.

This PRD defines the **Quality Intelligence Platform (QIP)** — a two-sided system that:
1. **Detects and resolves quality failures proactively** at the warehouse/delivery level.
2. **Identifies patterns in customer queries** to systematically eliminate the root causes of recurring complaints.

The platform bridges Operations, Customer Support, and Product teams with a shared data layer — turning reactive complaint management into proactive quality engineering.

---

## 2. Problem Statement

### 2.1 Current State

Quick commerce platforms (Blinkit, Zepto, Swiggy Instamart, etc.) face a structural tension: **speed vs. quality**. Dark store pickers work under time pressure, which leads to:

- Wrong SKUs being picked
- Damaged or crushed items (especially fresh produce, eggs, chips)
- Products close to or past expiry being dispatched
- Incorrect quantities or missing items
- Poor thermal management for temperature-sensitive goods

These failures create a surge in customer queries — most of which are repetitive, pattern-driven, and solvable at the source.

### 2.2 The Core Problem

> **"Customers are raising the same complaints repeatedly because the same quality failures happen repeatedly — and no system connects the complaint signal back to the source of failure."**

| Symptom | Root Cause |
|--------|-----------|
| High CSAT ticket volume | No proactive quality check at pick/pack stage |
| High refund rate | Wrong or damaged items shipped without detection |
| Agent resolution time is high | Queries aren't auto-classified or pre-diagnosed |
| Same queries reappear weekly | No feedback loop from support to ops |
| Customer churn post-bad experience | No proactive resolution before customer complains |

### 2.3 The Opportunity

If we can:
- **Tag and cluster** every incoming query by type, SKU, category, and dark store
- **Surface patterns** to ops and warehouse teams in near-real-time
- **Auto-resolve** high-confidence, simple queries (missing item, quantity error)
- **Prevent** recurring failures with quality checkpoints at pick/pack

...we reduce complaint volume, improve CSAT, and protect LTV.

---

## 3. Goals & Success Metrics

### 3.1 Business Goals

| Goal | Description |
|------|-------------|
| Reduce quality-related ticket volume | Fewer complaints = fewer failures at source |
| Increase CSAT score | Faster, more accurate resolution |
| Reduce refund rate | Catch errors before dispatch |
| Reduce repeat complaint rate | Break the cycle of recurring failures |
| Improve customer retention post-issue | Proactive compensation before customer asks |

### 3.2 Key Metrics (KPIs)

| Metric | Baseline (Estimated) | Target (6 months) |
|--------|---------------------|-------------------|
| Quality-related ticket volume | 100% (index) | ↓ 35% |
| First Contact Resolution (FCR) rate | ~55% | ↑ 80% |
| Average Handle Time (AHT) | ~6 min | ↓ 3.5 min |
| CSAT score | ~3.4/5 | ↑ 4.2/5 |
| Refund rate (quality issues) | ~12% of orders | ↓ 7% |
| Repeat complaint rate (same user, same issue) | ~18% | ↓ 5% |
| % queries auto-resolved | ~10% | ↑ 45% |

### 3.3 North Star Metric

> **% of orders completed with zero post-delivery quality complaints** (Order Quality Score)

---

## 4. User Personas

### 4.1 Primary Personas

**Persona 1: Rahul – The Time-Pressed Customer**
- Urban professional, 28–38 years old
- Orders groceries, snacks, household items via quick commerce 4–5x/week
- Pain: Receives wrong/damaged items; hates waiting on chat support; expects instant resolution
- Need: Fast acknowledgment, instant refund/replacement, no need to explain the same issue twice

**Persona 2: Priya – The Support Agent**
- Handles 80–120 chats/day
- Pain: Same query types repeat; no visibility into whether the issue is a one-off or systemic; slow lookup of order details
- Need: Auto-suggested resolution, pre-filled order context, pattern alerts for known issues

**Persona 3: Aditya – The Dark Store Manager**
- Manages 15–30 pickers across a 2,000–5,000 sq ft warehouse
- Pain: No visibility into which SKUs or picker behaviors generate complaints; gets blame after the fact
- Need: Real-time quality dashboards, picker performance scores, SKU-level defect alerts

**Persona 4: Neha – The Category/Ops Analyst**
- Analyzes trends, coordinates with suppliers and ops
- Pain: Support data is siloed; no structured way to link quality failures to SKUs or suppliers
- Need: Aggregated query pattern reports, root cause drill-downs, actionable ops recommendations

---

## 5. Market Context

### 5.1 Industry Pain Point

Across the quick commerce industry, quality-related complaints represent an estimated **25–40% of all support tickets**. The top complaint categories are:

1. Wrong item delivered
2. Missing item / incomplete order
3. Damaged/crushed packaging
4. Near-expiry or expired product
5. Quantity mismatch
6. Temperature abuse (melted ice cream, warm dairy)
7. Substituted product without consent

### 5.2 Existing Gaps

| Current Approach | Gap |
|-----------------|-----|
| Manual chat support | Slow, inconsistent, agent-dependent |
| Post-hoc refunds | Doesn't address repeat behavior |
| Periodic ops audits | Too infrequent, not data-driven |
| NPS surveys | Lagging indicator, low response rate |
| No query classification | Cannot identify patterns at scale |

---

## 6. Solution Overview

The **Quality Intelligence Platform (QIP)** has three integrated modules:

```
┌─────────────────────────────────────────────────────────────────────┐
│                    Quality Intelligence Platform (QIP)              │
│                                                                     │
│  ┌──────────────────┐  ┌───────────────────┐  ┌─────────────────┐  │
│  │  Query Pattern   │  │   Quality Control  │  │  Auto-Resolution│  │
│  │  Intelligence    │  │   Checkpoints      │  │  & Proactive    │  │
│  │  Engine (QPIE)   │  │   (QCC)            │  │  Alerts (APA)   │  │
│  │                  │  │                    │  │                 │  │
│  │  - ML tagging    │  │  - Pre-dispatch     │  │  - Auto-refund  │  │
│  │  - Clustering    │  │    photo capture   │  │  - Proactive    │  │
│  │  - Trend alerts  │  │  - Picker scoring  │  │    messaging    │  │
│  │  - Root cause    │  │  - Expiry checks   │  │  - Agent assist │  │
│  └──────────────────┘  └───────────────────┘  └─────────────────┘  │
│                                                                     │
│                 Shared Data Layer & Ops Dashboard                   │
└─────────────────────────────────────────────────────────────────────┘
```

---

## 7. Feature Requirements

### 7.1 Module A — Query Pattern Intelligence Engine (QPIE)

#### Feature A1: Auto-Classification of Incoming Queries

**Description:** Every incoming customer query (chat, call transcript, in-app report) is automatically tagged with structured labels.

**Taxonomy:**

```
Issue Type
├── Wrong Item
│   ├── Different Brand
│   ├── Different Variant (size/flavour)
│   └── Completely Wrong Product
├── Missing Item
│   ├── Partial Quantity
│   └── Entire Item Missing
├── Damaged
│   ├── Crushed Packaging
│   ├── Leakage/Spill
│   └── Broken Seal
├── Quality Issue
│   ├── Near Expiry (< 7 days)
│   ├── Past Expiry
│   ├── Stale/Spoiled
│   └── Temperature Abuse
└── Order-Level Issue
    ├── Wrong Order (different customer)
    └── Substitution (unconsented)
```

**Attributes tagged per query:**
- Query type (from taxonomy above)
- Linked SKU(s)
- Linked dark store ID
- Linked picker ID (if trackable)
- Order time slot
- Delivery partner

**Acceptance Criteria:**
- [ ] System classifies ≥ 90% of queries with correct primary tag
- [ ] Classification happens within 30 seconds of query submission
- [ ] Human override available for misclassified queries
- [ ] Override data used to retrain classifier monthly

---

#### Feature A2: Pattern Detection & Trend Alerts

**Description:** Aggregate classified queries to surface emerging patterns before they become crises.

**Logic:**
- If the same (Issue Type + SKU) combination appears ≥ 5 times in a 2-hour window from the same dark store → trigger a **Store-Level Alert**
- If the same (Issue Type + SKU) appears across ≥ 3 dark stores in a 6-hour window → trigger a **Systemic Alert** (supplier/category level)
- Daily digest report showing top 10 query patterns by volume and trend

**Alert Channels:**
- In-app dashboard notification
- WhatsApp/Slack message to dark store manager
- Email to category ops team (for systemic alerts)

**Acceptance Criteria:**
- [ ] Alert triggers within 15 minutes of threshold breach
- [ ] False positive rate < 10% (validated over 30-day period)
- [ ] Manager can acknowledge and add note to alert
- [ ] All alerts visible in centralized ops dashboard with status tracking

---

#### Feature A3: Root Cause Drill-Down Dashboard

**Description:** A self-serve analytics dashboard for ops and category teams.

**Key Views:**

| View | Description |
|------|-------------|
| Query Heatmap | Volume by store × issue type × day/hour |
| SKU Quality Scorecard | Complaint rate per SKU (complaints / units sold) |
| Picker Defect Rate | Errors linked to picker (wrong pick, missed item) |
| Supplier Pattern View | Complaint clusters aligned to supplier/batch |
| Resolution Effectiveness | Which resolutions reduced repeat complaints |

**Acceptance Criteria:**
- [ ] Dashboard loads in < 3 seconds
- [ ] Data refreshes every 15 minutes
- [ ] All views exportable to CSV
- [ ] Filterable by store, category, date range, issue type

---

### 7.2 Module B — Quality Control Checkpoints (QCC)

#### Feature B1: Pre-Dispatch Photo Capture

**Description:** Picker app captures a photo of the packed bag before handoff to delivery partner.

**Workflow:**
1. Picker completes packing
2. App prompts: "Capture packed bag photo"
3. Photo uploaded to order record
4. CV model flags potential issues (damaged packaging, visually wrong item)
5. Flagged orders reviewed by supervisor before dispatch

**Acceptance Criteria:**
- [ ] Photo captured for ≥ 95% of orders
- [ ] CV model achieves ≥ 80% precision in flagging damaged packaging
- [ ] Flagged orders reviewed within 3 minutes
- [ ] Photos retained for 30 days for dispute resolution

---

#### Feature B2: Expiry Date Scanner

**Description:** Picker scans or enters expiry date of perishable items at pick time.

**Rules:**
- Items with < 3 days to expiry: **Block pick** — must pick alternate batch
- Items with 3–7 days to expiry: **Warn picker** — customer receives notification of near-expiry before delivery
- Items with > 7 days: **Clear**

**Acceptance Criteria:**
- [ ] Scanner integrates with existing picker app with < 2 additional taps per item
- [ ] Block rule enforced — cannot complete order with blocked item
- [ ] Near-expiry notification sent to customer ≥ 5 minutes before delivery
- [ ] Expiry log maintained per SKU per store for audit

---

#### Feature B3: Picker Quality Score

**Description:** Each picker receives a rolling quality score based on order accuracy and complaint linkage.

**Score Components:**

| Factor | Weight |
|--------|--------|
| Wrong item picks (linked complaints) | 40% |
| Missing items | 30% |
| Photo quality (blur, missing bag) | 15% |
| Expiry rule violations | 15% |

**Score Bands:**

| Band | Score | Action |
|------|-------|--------|
| Green | 90–100 | No action |
| Yellow | 75–89 | Weekly coaching nudge in app |
| Orange | 60–74 | Manager review + targeted training |
| Red | < 60 | Performance improvement plan |

**Acceptance Criteria:**
- [ ] Score updates after each completed order
- [ ] Picker can view their own score and linked complaints in app
- [ ] Manager can view team scores and drill into individual orders
- [ ] Score not used punitively without manager review first (policy guardrail)

---

### 7.3 Module C — Auto-Resolution & Proactive Alerts (APA)

#### Feature C1: Intelligent Auto-Resolution

**Description:** For high-confidence, low-complexity queries, the system auto-resolves without agent involvement.

**Auto-Resolution Rules:**

| Condition | Auto Action |
|-----------|-------------|
| Missing item confirmed (CV photo shows missing bag slot) | Instant refund to wallet + apology message |
| Customer reports quantity error (< ₹200 order value) | Auto-credit to wallet, no proof required |
| Wrong item (customer photo submitted, confidence > 85%) | Replacement scheduled for next delivery |
| Expiry complaint for item flagged as near-expiry at pick | Auto-refund + escalate to supplier quality team |

**Guardrails:**
- Auto-resolution capped at ₹500 per order per customer per day
- Customers with > 5 auto-resolutions in 30 days flagged for review (abuse detection)
- All auto-resolutions logged for audit

**Acceptance Criteria:**
- [ ] Auto-resolution triggers within 2 minutes of query submission for eligible cases
- [ ] Customer notified via app push + in-chat message
- [ ] ≥ 80% customer satisfaction on auto-resolved queries (measured via post-resolution 1-tap rating)
- [ ] Agent can reverse auto-resolution within 24 hours if fraudulent

---

#### Feature C2: Proactive Customer Alerts

**Description:** Alert customers before they need to complain.

**Trigger Scenarios:**

| Trigger | Customer Message |
|---------|-----------------|
| CV model flags packaging damage post-dispatch | "We noticed your order may have a packaging issue. If anything looks wrong on delivery, reply 'ISSUE' here for instant resolution." |
| Near-expiry item dispatched | "Heads up: [Item Name] in your order has a best-before of [Date]. Use it soon!" |
| Dark store quality alert active for customer's order SKU | "We're keeping an eye on the quality of [Item Name] in your area. If anything seems off, let us know immediately." |

**Acceptance Criteria:**
- [ ] Message delivered ≥ 5 minutes before delivery ETA
- [ ] Customer can initiate resolution directly from alert message
- [ ] Proactive alerts reduce post-delivery complaints for flagged orders by ≥ 25%

---

#### Feature C3: Agent Assist Panel

**Description:** Support agents see a pre-filled context panel when handling quality complaints.

**Panel Contents:**
- Order summary + item photos (pre-dispatch + customer-submitted)
- Auto-suggested resolution based on query classification
- Historical resolution for this query type (what worked before)
- Customer lifetime value + complaint history (to calibrate resolution generosity)
- Similar active queries from same store (pattern indicator)

**Acceptance Criteria:**
- [ ] Panel loads within 1 second of query opening
- [ ] Agent accepts suggested resolution ≥ 70% of the time (adoption metric)
- [ ] AHT reduces by ≥ 40% for queries with agent assist vs. without

---

## 8. Query Pattern Intelligence Engine

### 8.1 Data Inputs

| Source | Data |
|--------|------|
| Chat transcripts | Free-text query, timestamp, order ID |
| In-app issue reporting | Structured issue type, photo, order ID |
| Call center transcripts | STT-converted text, call metadata |
| Picker app | Photo, expiry scan, pick confirmation |
| Order management system | SKU, quantity, store, picker, delivery partner |

### 8.2 ML Pipeline

```
Raw Query Input
     │
     ▼
Text Preprocessing (NLP cleaning, language detection, Hindi/Hinglish normalization)
     │
     ▼
Intent Classification (fine-tuned BERT/LLM-based classifier)
     │
     ▼
Entity Extraction (SKU name, quantity, issue descriptor)
     │
     ▼
Taxonomy Tag Assignment (multi-label: issue type + severity + category)
     │
     ▼
Order Record Linkage (join to order DB via order ID)
     │
     ▼
Aggregation Layer (store × SKU × time window pattern detection)
     │
     ▼
Alert Engine + Dashboard + Resolution Engine
```

### 8.3 Known Query Patterns & Recommended Resolutions

Based on industry data and quick commerce operational context, the following patterns and standard resolutions are pre-loaded:

| Query Pattern | Frequency (est.) | Recommended Resolution | Escalation Trigger |
|--------------|-----------------|----------------------|-------------------|
| Missing item (single item, < ₹300) | Very High | Auto-refund to wallet | None |
| Wrong item (brand mismatch) | High | Replacement at next delivery or refund | Customer declines replacement |
| Damaged packaging (dry goods) | High | Partial refund (30–50% of item value) | Customer escalates |
| Near-expiry produce | Medium | Full refund + proactive supplier alert | Systemic (3+ stores) |
| Expired product dispatched | Medium | Full refund + ₹50 goodwill credit | Always escalate to ops |
| Temperature abuse (ice cream, dairy) | Medium | Full refund | Systemic alert to cold chain team |
| Wrong order (different customer's bag) | Low | Full refund, reorder placed, privacy flag raised | Always escalate |
| Quantity short (e.g., 450g instead of 500g) | High | Partial refund (proportional) | None |
| Substitute without consent | Medium | Full refund of substituted item | None |

---

## 9. Quality Control Framework

### 9.1 Quality Gates at Dark Store Level

```
Order Placed
     │
     ▼
[Gate 1: Pick Verification]
 - Barcode scan confirms correct SKU
 - Expiry date check (block/warn)
 - Quantity confirmation
     │
     ▼
[Gate 2: Pack Verification]
 - Photo capture of packed bag
 - CV model check for damage/missing items
 - Weight validation (if scale integrated)
     │
     ▼
[Gate 3: Handoff Check]
 - Delivery partner confirms bag count
 - Thermal bag used for cold items (checkbox)
     │
     ▼
Dispatched → Customer
```

### 9.2 SLA Framework for Query Resolution

| Query Type | Auto-Resolution SLA | Agent SLA | Escalation SLA |
|-----------|--------------------|-----------|----|
| Missing item (confirmed) | 2 minutes | N/A | N/A |
| Wrong item (high confidence) | 5 minutes | N/A | N/A |
| Damaged product | 10 minutes | 15 minutes | 1 hour |
| Expiry complaint | 5 minutes | 10 minutes | 30 minutes |
| Complex/disputed | N/A | 20 minutes | 2 hours |

---

## 10. User Stories

### Customer Stories

> **US-01:** As a customer who received a missing item, I want the refund to be processed automatically without having to chat with an agent, so that I don't waste time explaining my issue.

> **US-02:** As a customer who received a near-expiry product, I want to be informed before delivery so I can decide whether to accept or cancel, so that I'm in control of what I put in my fridge.

> **US-03:** As a repeat customer who has had quality issues before, I want the system to remember my history and be more proactive with resolutions, so I feel valued and not like I have to fight every time.

### Support Agent Stories

> **US-04:** As a support agent, I want to see a pre-filled context panel when a quality complaint comes in, so that I can resolve it in under 3 minutes without asking the customer to repeat themselves.

> **US-05:** As a support agent, I want to see a suggested resolution based on the query type, so I can make consistent decisions aligned with company policy without memorizing every rule.

### Dark Store Manager Stories

> **US-06:** As a dark store manager, I want to receive an alert when the same quality complaint is reported 5+ times in 2 hours, so I can intervene with pickers immediately before more orders are affected.

> **US-07:** As a dark store manager, I want to see each picker's quality score on a dashboard, so I can have targeted coaching conversations backed by data.

### Ops/Category Team Stories

> **US-08:** As a category manager, I want to see which SKUs are generating the most quality complaints, so I can raise issues with suppliers and adjust sourcing decisions.

> **US-09:** As an ops analyst, I want to export query pattern data by date range and store, so I can include it in weekly ops reviews and track improvement over time.

---

## 11. Non-Functional Requirements

| Category | Requirement |
|----------|-------------|
| **Performance** | Query classification: < 30 seconds; Dashboard load: < 3 seconds |
| **Scalability** | Handle 10,000 concurrent queries during peak hours (evening, weekends) |
| **Availability** | 99.9% uptime SLA; no downtime during 6 PM–10 PM peak |
| **Data Retention** | Query logs: 12 months; Picker photos: 30 days; Scores: 6 months |
| **Privacy** | Customer data masked in dashboards; GDPR/DPDP Act compliant |
| **Security** | Role-based access control (customer, agent, manager, analyst, admin) |
| **Localization** | Support for English and Hindi query inputs; Hinglish normalization |
| **Accessibility** | Picker app: thumb-friendly UI, works on low-RAM Android devices |

---

## 12. Out of Scope

The following are explicitly excluded from this PRD and may be addressed in future iterations:

- Delivery partner quality tracking (separate domain)
- Supplier onboarding or procurement workflows
- Inventory management or demand forecasting
- Customer loyalty/rewards program integration
- Real-time GPS tracking of delivery
- Video recording at dark stores (separate privacy/legal workstream)

---

## 13. Dependencies & Risks

### 13.1 Dependencies

| Dependency | Owner | Risk if Delayed |
|-----------|-------|-----------------|
| Picker app instrumentation (photo capture, expiry scan) | Mobile Engineering | Gate 1 & 2 of quality control cannot launch |
| Order management system API access | Backend Engineering | Query-to-order linkage fails |
| Chat/call transcript pipeline | CX Tech | Classification engine has no input |
| CV model for packaging damage | ML Team | Photo-based auto-resolution unavailable |
| Dark store manager Slack/WhatsApp integration | Ops Tech | Alert delivery fails |

### 13.2 Risks

| Risk | Likelihood | Impact | Mitigation |
|------|-----------|--------|------------|
| Picker app adoption resistance | Medium | High | Change management, make score visible, gamify |
| CV model accuracy insufficient at launch | Medium | Medium | Launch with human-in-loop review; retrain iteratively |
| Auto-resolution fraud/abuse | Medium | Medium | Cap limits, abuse detection rules, manual review queue |
| Query volume spike overwhelming alert system | Low | High | Load test to 3x peak; circuit breakers in alert engine |
| Dark store manager ignores alerts | High | High | Manager SLA on alert acknowledgment; escalation to city ops head |

---

## 14. Timeline & Milestones

### Phase 1 — Foundation (Months 1–2)
- [ ] Query classification model v1 (top 5 issue types)
- [ ] Basic ops dashboard (volume by store × issue type)
- [ ] Agent assist panel v1 (order context + suggested resolution)
- [ ] Picker photo capture flow in app

### Phase 2 — Intelligence (Months 3–4)
- [ ] Pattern detection & alert engine live
- [ ] Expiry date scanner integrated in picker app
- [ ] Auto-resolution for missing items and quantity errors
- [ ] Picker quality score dashboard (manager view)

### Phase 3 — Proactive Quality (Months 5–6)
- [ ] Proactive customer alerts (pre-delivery notification)
- [ ] CV model for packaging damage detection
- [ ] Systemic alert escalation to category/supplier teams
- [ ] Root cause drill-down dashboard (SKU, picker, supplier views)
- [ ] Full auto-resolution suite across all eligible query types

---

## 15. Open Questions

| # | Question | Owner | Due |
|---|----------|-------|-----|
| 1 | What is the legal stance on using picker photos as performance evidence? | Legal | Month 1 |
| 2 | Can we link delivery partner handoff data to order record in real-time? | Backend Eng | Month 1 |
| 3 | What is the auto-resolution financial authority threshold approved by Finance? | Finance | Month 1 |
| 4 | Will dark store managers have Android or web dashboard access? | Ops | Month 2 |
| 5 | How do we handle multilingual (Kannada, Tamil, etc.) query inputs in Phase 2? | ML Team | Month 3 |
| 6 | What is the consent flow for near-expiry notifications to customers? | Legal/CX | Month 2 |

---

## Appendix A — Query Taxonomy (Full)

```
Quality Complaint
├── Product Issue
│   ├── Wrong Item
│   │   ├── Wrong Brand (e.g., Amul vs Britannia)
│   │   ├── Wrong Variant (e.g., Full Fat vs Toned)
│   │   ├── Wrong Size/Weight (e.g., 1L instead of 500ml)
│   │   └── Completely Wrong Product (e.g., shampoo instead of chips)
│   ├── Missing Item
│   │   ├── Entirely Missing
│   │   └── Partially Missing (quantity short)
│   ├── Damaged
│   │   ├── Crushed Packaging
│   │   ├── Leakage / Spillage
│   │   ├── Broken Seal
│   │   └── Physically Broken Product
│   ├── Quality/Freshness
│   │   ├── Expired Product
│   │   ├── Near Expiry (< 7 days)
│   │   ├── Stale / Wilted (for produce)
│   │   ├── Spoiled / Bad Smell
│   │   └── Temperature Abuse (melted, warm, frozen incorrectly)
│   └── Substitution
│       ├── With Consent (but customer unhappy)
│       └── Without Consent
└── Order Issue
    ├── Wrong Bag (different customer's order)
    ├── Duplicate Charge
    └── Missing Bag (partial order delivered)
```

---

## Appendix B — Glossary

| Term | Definition |
|------|-----------|
| **QIP** | Quality Intelligence Platform — the product defined in this PRD |
| **QPIE** | Query Pattern Intelligence Engine — Module A |
| **QCC** | Quality Control Checkpoints — Module B |
| **APA** | Auto-Resolution & Proactive Alerts — Module C |
| **Dark Store** | Micro-warehouse used for quick commerce fulfillment |
| **Picker** | Warehouse staff who picks items from shelves for each order |
| **FCR** | First Contact Resolution — % of queries resolved in first interaction |
| **AHT** | Average Handle Time — avg. time agent spends on each query |
| **CSAT** | Customer Satisfaction Score |
| **CV Model** | Computer Vision model used to inspect product/packaging photos |
| **SKU** | Stock Keeping Unit — unique identifier for each product variant |
| **LTV** | Customer Lifetime Value |
| **SLA** | Service Level Agreement — committed response/resolution time |

---

*This PRD is a living document. Updates will be tracked via Git commit history.*

*For questions or contributions, open a GitHub Issue or tag the PM in the relevant Slack channel.*
