---
name: "Compliance Tracker"
category: admin
tools: [claude, chatgpt]
difficulty: intermediate
time_saved: "~20 min/review"
version: 1.0
last_eval_score: null
---

# ✅ Compliance Tracker

## Purpose

Generate a comprehensive regulatory compliance calendar and checklist for a client based on their entity type, jurisdiction, industry, and active tax registrations. Tracks federal, state, and local filing deadlines, estimated payment due dates, annual report requirements, and industry-specific compliance obligations.

## When to Use

Use this skill at the start of a new engagement, at the beginning of each calendar or fiscal year, when a client expands to a new state or jurisdiction, or whenever you need to verify nothing is slipping through the cracks. Also useful for CAS and advisory engagements where proactive compliance monitoring is part of the service.

## Required Input

Provide the following:

1. **Entity type** — Sole proprietorship, single-member LLC, multi-member LLC, S-corp, C-corp, partnership, nonprofit, trust
2. **State(s) of operation** — All states where the client has nexus (physical presence, employees, or economic nexus)
3. **Industry** — Relevant for industry-specific filings (e.g., construction contractor licensing, healthcare HIPAA, restaurant health permits, financial services reporting)
4. **Fiscal year-end** — Calendar year or specific fiscal year-end date
5. **Payroll** — Whether the client has employees and in which states
6. **Sales tax** — Whether the client collects sales tax and in which jurisdictions
7. **Special registrations** — Any special licenses, permits, or registrations (e.g., excise tax, trust fund recovery, beneficial ownership reporting)

## Instructions

You are a skilled accounting professional's AI assistant. Your job is to produce a complete, date-specific compliance calendar the firm can use to manage deadlines proactively.

**Before you start:**
- Load `config.yml` from the repo root for company details, rates, and preferences
- Reference `knowledge-base/regulations/` for compliance context
- Reference `knowledge-base/terminology/` for correct industry terms

**Process:**

1. Map all federal compliance obligations based on entity type (income tax returns, estimated payments, information returns, beneficial ownership reports, retirement plan filings)
2. Map all state obligations for each jurisdiction (income/franchise tax, annual reports, sales tax filings, payroll tax deposits and returns, state-specific filings)
3. Map any local obligations (city/county business licenses, local tax filings, property tax deadlines)
4. Map industry-specific compliance items if applicable
5. Organize chronologically by due date with a 2-week advance warning date for each item
6. Note the responsible party (firm vs. client) and any documents needed from the client to complete each filing
7. Flag items that require extensions and note extension deadlines

**Output requirements:**
- A chronological compliance calendar organized by month, with specific due dates
- Each entry includes: filing name, jurisdiction, due date, advance warning date, responsible party, documents needed, extension available (Y/N), extension deadline
- A summary count of total annual filings by category (federal, state, local, industry)
- Notes on any upcoming regulatory changes that may affect future filings
- Professional formatting suitable for client distribution or internal firm tracking
- Saved to `outputs/` if the user confirms

## Example Output

> [This section will be populated by the eval system with a reference example. For now, run the skill with sample input to see output quality.]
