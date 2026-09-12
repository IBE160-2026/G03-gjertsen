---
title: Autonomous Consultant Invoicing & Planning Assistant
status: draft
created: 2026-09-13
updated: 2026-09-13
---

# Product Brief: Autonomous Consultant Invoicing & Planning Assistant

## Executive Summary

Consultants working in agile teams are measured on a simple number — how much of their working time is billable — but keeping that number healthy requires constant, tedious bookkeeping: checking hours logged, remembering invoicing targets, and guessing whether next month's project mix will actually hit the mark. Today this is manual, reactive work, usually noticed only when a shortfall has already happened.

This project is a **per-consultant autonomous AI agent** that watches a consultant's invoicing rate against their target (a yearly goal, 80% by default, individually overridable), keeps itself current with minimal input from the user, and proactively warns when the upcoming month looks like it will fall short. Rather than making consultants type status reports, it asks short, selectable-option questions about what they expect to work on next, and turns that into concrete hints for closing the gap. Leaders get the same picture rolled up across their team.

The idea comes directly from the author's own consulting experience, and is scoped for this course as a trial: the agent logic, dashboards, and planning experience are built first against simulated data standing in for the real system, with Tripletex (a Norwegian accounting/invoicing platform with a documented, freely accessible API) as the intended real data source for a later phase.

## The Problem

Consultants who bill by the hour are typically held to a utilization target (e.g. 80% of working hours invoiced). In practice:

- The number is checked **after the fact**, usually at month-end, when it's too late to act on a shortfall.
- Because the target is a **yearly** average, a slow week doesn't feel urgent in the moment — until several slow weeks compound and the deficit becomes hard to recover.
- Planning what to work on next is disconnected from the target: consultants pick up work based on what's available or asked of them, not on what would actually close their invoicing gap.
- Existing time-tracking/invoicing tools (like Tripletex) record what already happened; they don't tell you what to do next.
- Manually reviewing this — logging in, checking hours, doing the math, deciding what to prioritize — is tedious enough that it mostly doesn't happen until a leader asks about it.

The cost is twofold: consultants lose income-relevant billable time they can't recover, and leaders discover utilization problems late, across a whole team at once, instead of catching them early per person.

## The Solution

An AI agent assigned **one per consultant** that:

1. **Tracks continuously**, not just at reporting time — keeping itself up to date on hours logged versus the consultant's yearly invoicing target.
2. **Notices proactively**: if the trajectory suggests an upcoming month won't hit the target, it raises a notice before the month happens, not after.
3. **Asks, rather than demands typing**: instead of a form to fill out, it asks the consultant what they expect to work on in the near future using selectable options (e.g. client/project, expected hours per day across a standard Mon–Fri, 7.5h/day baseline).
4. **Turns that into hints**: given the declared upcoming plan, it tells the consultant where there's room to add billable work to reach their target — a lightweight planning nudge, not a rigid schedule.
5. **Rolls up for leaders**: a team-level dashboard gives leaders the same forward-looking picture across all their consultants, without needing to ask each one individually.

## Who This Serves

**Consultants (primary user, and the AI's direct owner)**
Each consultant has their own personal dashboard and their own instance of the agent, tuned to their own target. They want to know, with almost no effort on their part, whether they're on track — and if not, what to do about it. Success for them looks like: fewer surprises at month-end, less manual checking, and a target that feels achievable because they were nudged early.

**Leaders / team leads (secondary user)**
Leaders need visibility across the whole team without chasing individuals for status. Their dashboard aggregates the same forward-looking signal per consultant into a team view, so at-risk utilization surfaces on its own.

## Success Criteria

- A consultant can see, at a glance, whether they're on pace for their yearly invoicing target — without manually calculating anything.
- The agent surfaces a below-target warning **before** the shortfall month arrives, not after.
- Declaring upcoming work takes selectable-option interaction, not free-text data entry.
- The system correctly computes a rolling/yearly utilization pace (not a naive weekly average) against each consultant's own target (default 80%, overridable).
- Leaders can see team-wide forward-looking utilization risk from a single dashboard.
- [ASSUMPTION] Course deliverable success = a working end-to-end trial (simulated data → dashboard → proactive hint) demonstrable live, since real Tripletex integration is out of scope for this phase.

## Scope

**In scope for this trial (Phase 1):**
- Per-consultant dashboard showing current pace toward yearly invoicing target.
- Per-leader dashboard aggregating team members' pace.
- Selectable-option flow for a consultant to declare expected upcoming work (client/project, expected hours per day).
- Hint-generation logic: given declared upcoming work + current pace, suggest where there's room to reach the target.
- Proactive under-target notice for an upcoming month.
- Configurable per-consultant target, defaulting to 80% org-wide.
- Data layer built against **simulated/sample data** shaped like Tripletex's model (timesheets, projects, employees), so it can be swapped for the real API later without a redesign.

**Explicitly out of scope for this trial:**
- Live connection to the real Tripletex API (confirmed feasible for later — see Feasibility Note below — but not built now).
- Multi-tenant support for multiple companies (Tripletex's commercial-partner approval path is irrelevant at this stage).
- [ASSUMPTION] Notification delivery channel (in-app only vs. email/Slack) is undecided — default assumption is in-app for the trial.
- Any billing/invoice-generation functionality itself — this project only observes and advises, it does not create invoices.

**Feasibility note (Phase 2 / future):** Tripletex publishes a documented REST API (API 2.0) with a free, self-serve test sandbox and no partner-approval requirement for single-company use — only multi-tenant commercial integrations need the longer approval path. This means the real integration this project is ultimately aimed at is realistically achievable without institutional or paid access. An existing open-source project (tripletex-mcp) already demonstrates exposing Tripletex timesheet/project/invoice data to AI assistants, and is worth reviewing as reference architecture when Phase 2 begins.

## What Makes This Different

Time-tracking and invoicing tools (Tripletex included) are systems of record: they tell you what already happened. This project is a layer on top that is forward-looking and low-friction — it asks instead of demanding entry, and it warns before the number is already bad rather than reporting it after. The per-consultant (rather than one shared org-level) agent design means the advice is personal to that consultant's own target and situation, not a generic team-wide report.

## Vision

If this proves out, the natural next step is the live Tripletex connection (technically de-risked already), turning the trial into a tool a real consulting team could use day to day. Beyond that, the same pattern — proactively asking a person what they expect to do, and turning it into a nudge toward a target — could extend past invoicing into broader capacity and workload planning for agile teams generally.
