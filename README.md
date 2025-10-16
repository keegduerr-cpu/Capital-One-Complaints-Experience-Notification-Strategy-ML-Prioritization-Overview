# Capital One — Complaints Experience: Notification Strategy & ML Prioritization

> [!TIP]
> This README preserves every word of your original write-up. Only formatting, a Table of Contents, and an optional diagram have been added for GitHub readability.

## Table of Contents
- [Overview](#overview)
- [My Role as Product Manager](#my-role-as-product-manager)
- [Product Principles](#product-principles)
- [Context: Why Complaints Matter](#context-why-complaints-matter)
- [Objective](#objective)
- [Insights That Shaped the Approach](#insights-that-shaped-the-approach)
- [The Two-Pronged Solution](#the-two-pronged-solution)
  - [1) Detect & Create the Missing Complaint (ML + Case Management)](#1-detect--create-the-missing-complaint-ml--case-management)
  - [2) Acknowledge Fast with Smart, Targeted Notifications](#2-acknowledge-fast-with-smart-targeted-notifications)
- [Results](#results)
- [What I Owned](#what-i-owned)
- [Takeaways](#takeaways)
- [System Flow (Mermaid)](#system-flow-mermaid)

---

## Overview

This summary outlines a key product decision I led in Capital One’s complaints domain: implementing proactive customer notifications and an ML-driven model to detect, create, and prioritize complaints in order to reduce regulatory escalations and improve customer experience.

## My Role as Product Manager

At Capital One, product managers are accountable owners of an “area of intent.” I owned ~70% of our Extended Servicing portfolio across two teams, partnering closely with design, engineering, data science, operations, and risk. My responsibilities spanned:

- Defining product vision and aligning stakeholders up to VP level
- Annual/quarterly/sprint prioritization and roadmap ownership
- Risk ownership (data, access, incidents) for my portfolio
- End-to-end delivery: discovery, current-state analysis, requirements, UI design, Jira planning, change management, rollout, QA/UAT, and continuous improvement

## Product Principles

- Design decisions begin with the customer. Internally we track time, throughput, and cost; customers experience delight, clarity, and pain. I anchor every choice to the customer POV.
- Hold the vision steady; flex the details. Ship, learn, and iterate rather than stall in indecision.
- Prioritize ruthlessly. Value clarity—and bring even disappointed stakeholders along with the strategy.

## Context: Why Complaints Matter

Banks must intake, store, and sometimes report complaints to regulators (e.g., OCC). High complaint volumes—especially with regulatory implications—hurt reputation and can create material risk. I view complaints as a chance to transform negative moments into trust-building experiences; treating them only as a compliance checkbox is a missed opportunity.

## Objective

Improve customer experience by increasing transparency and speed of acknowledgment while reducing regulatory escalations. The hypothesis: better, faster acknowledgment and routing would lower escalation risk and strengthen trust.

## Insights That Shaped the Approach

- Historical patterns showed customers who had already complained were nearly 3× more likely to escalate; roughly 80% of regulatory complaints had prior bank complaint activity. This suggested substantial pre-escalation interception potential.
- Behavioral research indicated that when customers feel wronged, acknowledgment matters more than debate. Attempting to prove “no wrongdoing” escalates friction; prompt recognition reduces it.

## The Two-Pronged Solution

- Detect & create the “missing complaint”
- Acknowledge fast with smart notifications

### 1) Detect & Create the Missing Complaint (ML + Case Management)

- Partnered with Data Science to build an ML classifier that scans call transcripts to detect likely complaint content.
- After a short logging window for associates to file a complaint, our system checked for a matching submission.
- If none existed, we auto-created a complaint in the case management system using ML outputs and call metadata.
- A companion sentiment/escalation model flagged high-risk cases. Those complaints were surfaced higher in dynamic prioritization, accelerating outreach and resolution.

### 2) Acknowledge Fast with Smart, Targeted Notifications

- Designed customer notifications triggered when we detected a complaint, acknowledging the experience without admitting fault in unknown scenarios.
- Drafted copy and secured approvals across Marketing, Legal, and Risk; tuned language to acknowledge harm while avoiding confusion or liability.
- Implemented channel logic: push notification via mobile app first; if no app usage detected, send email.
- Excluded edge populations (e.g., internal-only complaints filed on a customer’s behalf) where proactive messages would confuse and spike call volume.

## Results

- The combined approach both captured unreported complaints and moved acknowledgment from ~8 days to near-immediate, materially improving perceived responsiveness.
- Regulatory complaint escalations fell by ~12% in the subsequent month—a program-best reduction at the time.
- In addition to pre/post analysis, we ran A/B tests on notification copy, timing, and channel; insights informed ongoing iteration (engagement, resolution time, and escalation rate as key metrics).

## What I Owned

- Strategy and hypothesis framing; success metrics and guardrails
- Cross-functional alignment (Ops, DS/ML, Engineering, Design, Compliance, Legal, Risk)
- Requirements, UI/UX for case handling flows, prioritization rules, and notification triggers
- Rollout plan, change management, training support, and post-launch iteration

## Takeaways

- Acknowledge early, route intelligently. Speed + empathy lowers escalation risk and builds trust.
- ML is most valuable when embedded in workflow. Detection alone isn’t enough; auto-creation and prioritization drove the outcome.
- Guardrails matter. Opt-outs and population controls prevent confusion and protect capacity.
- Measure what customers feel, not just what teams ship. CX-aligned metrics correlated with risk reduction.

---

## System Flow (Mermaid)

```mermaid
flowchart TD
  A[Call transcript] --> B[ML classifier: complaint likelihood]
  B -->|threshold met| C{Existing complaint?}
  C -->|yes| D[No action]
  C -->|no| E[Auto-create complaint case]
  E --> F[Sentiment / escalation model]
  F --> G[Dynamic prioritization queue]
  G --> H[Associate outreach & resolution]
  E --> I[Trigger notification: push → email fallback]
  I --> H
