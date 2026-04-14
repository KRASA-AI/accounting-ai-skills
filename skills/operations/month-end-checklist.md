---
name: "Month-End Checklist"
category: operations
tools: [claude, chatgpt]
difficulty: intermediate
time_saved: "~15 min/client"
version: 2.0
last_eval_score: 8.1
---

# ✅ Month-End Checklist

## Purpose

Generate a comprehensive, client-specific month-end close checklist tailored to the client's entity type, industry, chart of accounts, and accounting software — covering every reconciliation, adjustment, and review step needed to produce accurate financials.

## When to Use

Use this skill at the start of a new bookkeeping or CAS engagement to establish the close process, when onboarding a new staff member to a client, when a client's business has changed (new entity, new state, new revenue streams), or whenever you want to systematize and document the monthly close procedure for a specific client.

## Required Input

Provide the following:

1. **Client details** — Business name, entity type (sole prop, LLC, S-corp, C-corp, partnership, nonprofit), and industry
2. **Accounting software** — QuickBooks Online, QuickBooks Desktop, Xero, Sage, FreshBooks, or other
3. **Accounting basis** — Cash, accrual, or modified cash
4. **Key account types present** — Which of these apply: cash/bank accounts, accounts receivable, inventory, fixed assets, prepaid expenses, accounts payable, credit cards, loans/notes payable, payroll, sales tax, intercompany accounts, deferred revenue
5. **Revenue model** — How the client earns revenue (product sales, service billing, subscription, project-based, mixed)
6. **Payroll** — In-house, outsourced (to whom), or no employees
7. **State(s) of operation** — For sales tax and payroll tax reconciliation steps
8. **Special considerations** — Multi-entity consolidation, foreign currency transactions, grant reporting, construction WIP, trust accounting, or other industry-specific items

## Instructions

You are a skilled accounting professional's AI assistant. Your job is to produce a thorough, sequenced month-end close checklist that a bookkeeper or staff accountant can follow step by step to close the books accurately and completely.

**Before you start:**
- Load `config.yml` from the repo root for company details, rates, and preferences
- Reference `knowledge-base/terminology/` for correct industry terms
- Use the company's communication tone from `config.yml` → `voice`

**Process:**

Build the checklist in the standard close sequence. Include only sections relevant to the client's account types and entity:

1. **Pre-close preparation**
   - Confirm all bank and credit card transactions are downloaded/imported through month-end
   - Verify all client-provided documents are received (bank statements, credit card statements, loan statements)
   - Confirm payroll has been posted for the month

2. **Bank and credit card reconciliation**
   - Reconcile each bank account to the statement balance
   - Reconcile each credit card to the statement balance
   - Investigate and resolve outstanding reconciling items older than 30 days

3. **Revenue and receivables**
   - Review and post all revenue entries for the period
   - Review AR aging — flag balances over 60/90 days and confirm bad debt reserve is adequate
   - For accrual-basis: post unbilled revenue accruals; reverse prior-month accruals

4. **Expenses and payables**
   - Review AP aging for completeness
   - For accrual-basis: accrue known expenses not yet billed (utilities, professional services, etc.)
   - Verify payroll expense reconciles to payroll reports (gross wages, employer taxes, benefits)
   - Reconcile sales tax collected to sales tax payable; confirm returns filed

5. **Balance sheet account reconciliations**
   - Inventory: reconcile to count or adjust per physical count (if applicable)
   - Prepaids: amortize insurance, rent, and other prepaid balances
   - Fixed assets: record additions, disposals, and run monthly depreciation
   - Loans: reconcile balances to lender statements; verify interest expense
   - Intercompany: reconcile intercompany balances (must net to zero)
   - Deferred revenue: recognize the earned portion for the period

6. **Payroll tax reconciliation**
   - Verify 941/940 liability balances match payroll provider reports
   - Confirm state withholding and SUI/SDI deposits are current
   - Reconcile payroll tax liability accounts

7. **Sales tax reconciliation**
   - Confirm sales tax collected matches sales tax payable account
   - Verify filings are current for each jurisdiction

8. **Journal entries and adjustments**
   - Post all recurring journal entries (depreciation, amortization, allocations)
   - Post non-recurring adjustments with supporting documentation
   - Review prior-period adjustments — ensure prior-month entries were not altered

9. **Review and quality control**
   - Run trial balance and scan for unusual balances (negative assets, negative revenue)
   - Compare key line items to prior month and prior year — investigate variances exceeding 10%
   - Review unclassified or "Ask My Accountant" transactions — reclassify all
   - Verify retained earnings / equity rollforward is clean

10. **Close the period**
    - Set the close date in the accounting software to prevent changes
    - Export or save final trial balance and financial statements
    - Note carryover items for next month

**Output requirements:**
- Numbered checklist with checkboxes, grouped by the sections above
- Skip sections not relevant to the client (e.g., no inventory for a service business, no intercompany for a single entity)
- Each step should be specific enough that a staff accountant knows exactly what to do
- Include a "Documents Needed from Client" summary at the top
- Note estimated time for each section based on client complexity
- Professional formatting suitable for a recurring monthly procedure document
- Saved to `outputs/` if the user confirms

## Example Output

> [This section will be populated by the eval system with a reference example. For now, run the skill with sample input to see output quality.]
