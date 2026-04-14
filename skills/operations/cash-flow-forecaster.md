---
name: "Cash Flow Forecaster"
category: operations
tools: [claude, chatgpt]
difficulty: intermediate
time_saved: "~30 min/forecast"
version: 1.0
last_eval_score: null
---

# 📊 Cash Flow Forecaster

## Purpose

Build a rolling 13-week cash flow projection from current balances, expected receivables, and planned disbursements. Highlights weeks where cash may dip below a safety threshold and flags collection risks.

## When to Use

Use this skill when a client needs short-term liquidity visibility — before a line-of-credit draw, during seasonal revenue dips, when evaluating a large purchase, or as part of a recurring weekly treasury review. Also useful for CAS (Client Accounting Services) engagements that include cash management advisory.

## Required Input

Provide the following:

1. **Current cash position** — Bank balances as of the projection start date
2. **Accounts receivable aging** — Open invoices with expected collection dates (or a standard aging schedule)
3. **Accounts payable schedule** — Known upcoming disbursements (rent, payroll, vendor payments, loan payments, tax deposits)
4. **Recurring revenue/expenses** — Any predictable weekly or monthly inflows and outflows
5. **One-time items** — Planned capital expenditures, tax payments, distributions, or other non-recurring cash events
6. **Minimum cash threshold** — The balance the client wants to stay above (safety buffer)

## Instructions

You are a skilled accounting professional's AI assistant. Your job is to produce a clear, week-by-week cash flow projection that a business owner or CFO can act on.

**Before you start:**
- Load `config.yml` from the repo root for company details, rates, and preferences
- Reference `knowledge-base/terminology/` for correct industry terms
- Use the company's communication tone from `config.yml` → `voice`

**Process:**

1. Organize all inflows and outflows into weekly buckets across the 13-week horizon
2. For receivables without specific expected dates, apply standard collection assumptions (e.g., 70% collected within terms, 20% 15 days late, 10% 30+ days late — adjust based on client history if provided)
3. Calculate the ending cash balance for each week: Opening Balance + Inflows − Outflows = Ending Balance
4. Flag any week where the ending balance falls below the minimum cash threshold
5. Identify the peak cash need (lowest projected balance) and the week it occurs
6. Provide a brief narrative summary covering overall trajectory, key risk weeks, and recommended actions

**Output requirements:**
- A 13-week table with columns: Week #, Week Ending Date, Beginning Balance, Cash Inflows, Cash Outflows, Net Change, Ending Balance
- Color-code or mark weeks that breach the minimum threshold
- A summary section with key findings and recommended actions (e.g., accelerate collections, defer discretionary spending, arrange credit facility)
- Professional formatting appropriate for a client-facing deliverable
- Saved to `outputs/` if the user confirms

## Example Output

> [This section will be populated by the eval system with a reference example. For now, run the skill with sample input to see output quality.]
