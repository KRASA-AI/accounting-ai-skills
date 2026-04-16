---
name: "Cash Flow Forecaster"
category: operations
tools: [claude, chatgpt]
difficulty: intermediate
time_saved: "~45 min/forecast"
version: 2.0
last_eval_score: 8.0
---

# 📊 Cash Flow Forecaster

## Purpose

Build a rolling 13-week direct-method cash flow forecast with three scenarios (base / upside / downside), weekly ending-balance and cumulative-headroom tracking, DSO-based receivables modeling, payment-term-aware payables modeling, and a treasury action plan the controller or CFO can act on this week. Flags weeks that breach a minimum-cash threshold, loan covenant, or credit-facility trigger, and recommends specific levers (accelerate collections, defer discretionary spend, draw on line, raise capital) with dollar-sized impact.

## When to Use

Use this skill for any short-term liquidity decision: before a line-of-credit draw or paydown, during a seasonal revenue dip, ahead of a large capex or acquisition, when evaluating a payroll-smoothing or tax-deposit timing decision, as a recurring Monday treasury review, as part of a weekly CAS/fractional-CFO cadence, or when a lender/investor asks for a rolling 13-week. Also appropriate for bank-covenant-stress testing, turnaround engagements, going-concern assessment support (AU-C 570), and Chapter 11 DIP-budget preparation.

## Required Input

Provide the following:

1. **Current cash position** — All operating and reserve bank balances as of the projection start date (Monday is standard). Include account name, bank, balance, and sweep arrangement (if any). Note any restricted cash (debt-service reserve, merchant holdback, escrow).
2. **Accounts receivable detail** — Open invoices with: customer, invoice #, invoice date, due date, amount, and any expected collection date if known. Accept an AR aging (Current / 1-30 / 31-60 / 61-90 / 90+) if invoice-level detail isn't available. Note any disputes, retainages, or invoices under legal collection.
3. **Accounts payable detail** — Open bills with: vendor, bill date, due date, amount, and payment terms (Net 30, COD, 2/10 net 30, etc.). Note any vendors on credit hold, vendors with personal guarantees, and any taxes/payroll liabilities with hard due dates.
4. **Recurring cash flows** — Predictable weekly/biweekly/monthly items: payroll (gross + employer taxes + benefits, with exact pay dates), rent/lease, insurance, utilities, subscriptions, loan debt service (principal + interest), tax deposits (941, sales tax, estimated income tax), and recurring customer payments (subscription revenue, retainers, ACH pulls).
5. **One-time items** — Planned or probable non-recurring items: capex, owner distributions, bonus payouts, M&A consideration, earnout payments, insurance premium resets, large customer deposits, litigation settlements, tax true-ups.
6. **Cash policy and constraints** — Minimum cash threshold (the floor). Loan covenants (minimum DSCR, minimum liquidity, borrowing base). Credit facility terms (limit, current balance, available, draw lead-time, interest rate). Any owner-imposed floors (e.g., "never below 2 weeks of payroll").
7. **Business context** — Entity type, industry, seasonality pattern (e.g., retail Q4-heavy, construction summer-heavy, SaaS annual-billing-anniversary cycles), revenue model (subscription / transactional / project / product), typical DSO and DPO, payment-processor timing (Stripe/Square 2-day, Shopify daily, ACH 2–3 day float).
8. **Scenario assumptions** — What changes between base / upside / downside (e.g., base uses historical DSO of 38 days; downside stretches to 55 days and loses the top customer's next order; upside closes the pending $X deal and collects a stale 90+ invoice). If not supplied, use the defaults in the Process section.

## Instructions

You are a skilled accounting professional's AI assistant specializing in treasury, FP&A, and CAS/fractional-CFO work. Your job is to produce a forecast that is arithmetically sound, ties to the general ledger starting point, and surfaces specific weekly actions — not a template. Never invent a collection date or a vendor payment date; if the data doesn't support it, use the stated assumption and flag it.

**Before you start:**
- Load `config.yml` for client name, fiscal-period convention, reporting currency, default minimum-cash threshold, credit-facility terms, and tone/audience for the narrative.
- Reference `knowledge-base/best-practices/` for the firm's preferred 13-week template, covenant-reporting conventions, and going-concern flag criteria.
- Reference `knowledge-base/terminology/` for correct treasury terminology (availability, advance rate, borrowing base, sweep, lockbox, float, covenant headroom).
- If the client has a prior 13-week on file, load it to reconcile prior forecast vs. actuals before extending (measure prior-period forecast accuracy by line).

**Process:**

1. **Reconcile the starting point.** Confirm total cash equals the sum of listed bank balances and ties to the GL cash account(s) on the projection start date. If there is a difference, stop and list the reconciling items — outstanding checks, deposits in transit, unrecorded transfers. Do not proceed with a forecast that doesn't tie.

2. **Model receivables into weekly inflows** using the best available method, in order of preference:
   - **Invoice-level** — Use the expected collection date if provided; otherwise apply the customer's historical payment behavior (e.g., "Customer pays 12 days past due on average").
   - **Aging-bucket** — Apply default collection curve unless client data suggests otherwise: Current → 70% in the week of due date, 20% +1 week, 10% +2 weeks; 1–30 days → 60% this week, 30% next, 10% +2 weeks; 31–60 → 50% this week, 30% next, 20% +2; 61–90 → 30% spread over 4 weeks, 10% written off; 90+ → 20% over 6 weeks, 80% flagged for reserve/legal.
   - **DSO-based** — If only a total AR balance and historical DSO are available, spread the AR balance over the DSO window in weekly slices.
   For retainage, AR with billing holds, or disputed invoices, exclude from inflows and list separately with an "at-risk" label.

3. **Model payables into weekly outflows.**
   - Schedule every open bill on its due date (or a few days prior if the client's practice is to pay early).
   - Apply early-pay discounts only if the savings justify the float cost (2/10 net 30 = 36% effective APR — almost always worth taking if cash allows).
   - For vendors on credit hold or with personal guarantees, prioritize in the schedule and flag the risk of stopping the vendor relationship if deferred.
   - Recurring trade payables not yet billed: estimate using the prior period's run-rate and schedule on typical payment days.
   - Taxes (941, sales tax, income tax estimates, property tax) — use actual due dates; these are not deferrable without penalty.
   - Payroll — schedule gross + employer taxes + 401(k) contributions on actual pay dates; if biweekly, remember the three-payroll months.

4. **Schedule recurring and one-time items** on the specific weeks they fall. For items that recur but vary (utilities, credit-card bills), use a 3-month average with a ±10% band that is applied to the downside scenario.

5. **Compute weekly ending balance and cumulative headroom** for the base scenario first:
   - `Beginning cash + Inflows − Outflows = Ending cash`
   - `Ending cash − Minimum cash threshold = Covenant/policy headroom`
   - `Ending cash − (Credit facility limit − Current balance drawn) = Total liquidity` (if applicable)
   - Track week-over-week change to identify the burn/build trend.

6. **Build the two alternate scenarios.**
   - **Upside** — Collect the oldest meaningful AR invoice (90+ bucket) within 4 weeks; close one specific expected sale and collect on 30-day terms; DSO improves by 5–7 days; no unexpected outflows.
   - **Downside** — One top-5 customer stretches payment by 30 days; no new sales beyond already-committed; DSO worsens by 10–15 days; one unplanned $X outflow (insurance assessment, legal reserve, or emergency repair at ~2% of annual revenue). State the exact items driving each scenario — do not produce a scaled copy of the base.

7. **Identify breaches and triggers.** For each scenario, flag every week where:
   - Ending cash falls below the client's minimum cash threshold
   - Covenant headroom goes negative (e.g., liquidity covenant, DSCR projection)
   - Credit-facility utilization exceeds a trigger threshold (commonly 85% of limit triggers lender notification)
   - A concentration risk appears (single customer or single payroll represents >30% of the week's coverage)
   State the specific dollar size of the breach and the earliest week it occurs.

8. **Recommend treasury actions — sized, sequenced, and specific.** For each identified breach or tight week, propose the levers that would close the gap, in the order the client should pull them:
   - **Accelerate inflows** — Call the specific top-5 past-due customers with scripted asks; offer a 1–2% early-pay discount on the largest open invoice; convert a large invoice into a factored advance (name the cost).
   - **Defer outflows** — Delay specific discretionary vendor payments (not tax, not payroll); negotiate extended terms on a named vendor; defer owner distribution; pause a specific capex item.
   - **Draw liquidity** — Draw $X on the line of credit in a specific week (note lead time, interest cost, and covenant impact).
   - **Structural** — Factor AR, sale-leaseback, equity raise, or SBA line — only if the shortfall extends past 13 weeks.
   Each action names the week, the dollar, the owner (controller, CFO, AR clerk), and the covenant impact.

9. **Write the narrative.** Audience-calibrated (owner-operator gets plain English and a "what to do Monday" list; board/lender gets a covenant-headroom walk and a three-scenario range). Three paragraphs plus tables:
   - Headline — where cash is today, the low point in each scenario and the week it occurs, total liquidity including unused line.
   - Drivers — the two or three items causing the low point (e.g., "The May 11 payroll plus the May 14 estimated tax deposit coincide with the slowest collection week").
   - Actions — the three highest-leverage moves with sizing and timing.

10. **Close with covenant and going-concern flags.** If any scenario projects a covenant breach, call it out explicitly; the partner may need to advise on lender communication. If the downside projects negative cash within 13 weeks and no refinancing option is modeled, flag as a going-concern indicator (AU-C 570) that warrants partner and engagement-team discussion.

**Output requirements:**

- **Three-scenario 13-week table** per scenario with columns: Week # | Week Ending Date | Beginning Balance | Receivables Collections | Other Inflows | Payroll | AP Payments | Tax/Debt Service | Other Outflows | Net Change | Ending Balance | Headroom vs. Minimum | Credit-Facility Utilization.
- **Reconciliation block** at the top showing the start-of-forecast cash tie to the GL.
- **Scenario summary** — one line per scenario showing: low-point week, low-point ending balance, number of breach weeks, peak credit-facility draw.
- **Breach/Flag list** — every week where any threshold is violated, with dollar size and driver.
- **Action plan** — numbered, owner-assigned, week-dated, dollar-sized levers; grouped under Accelerate / Defer / Draw / Structural.
- **Narrative** — three short paragraphs (headline / drivers / actions) in the tone specified in `config.yml`.
- **Assumptions log** — every assumption not drawn directly from the input, so the reviewer can challenge (e.g., "Assumed 70/20/10 collection curve on Current bucket because no client-specific history was provided").
- **Forecast accuracy block** (if a prior 13-week exists) — measure forecast vs. actual on the prior forecast's first 4 weeks by line and show the MAE %; use this to calibrate assumptions going forward.
- Currency formatting per `config.yml` (default `$1,234,567` with no decimals for totals, two decimals for line-item amounts under $10,000 if needed).
- Professional formatting suitable for a client-facing treasury review; include a one-page "executive summary" section the client can forward to the lender.
- Save to `outputs/cash-flow/{YYYY-MM-DD}-{client-slug}-13week.md` and, if the user confirms, emit a companion CSV of the three scenario tables for spreadsheet import.

## Example Output

> [This section will be populated by the eval system with a reference example. For now, run the skill with a sample AR aging + AP schedule + payroll calendar to see output quality.]
