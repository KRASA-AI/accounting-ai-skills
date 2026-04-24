---
name: "Month-End Checklist"
category: operations
tools: [claude, chatgpt]
difficulty: intermediate
time_saved: "~25 min/client"
version: 2.1
last_eval_score: 8.1
---

# ✅ Month-End Checklist

## Purpose

Generate a comprehensive, client-specific, dependency-ordered month-end close checklist tailored to the client's entity type, industry, chart of accounts, and accounting software — covering every reconciliation, adjustment, and review step needed to produce accurate financials — with workpaper-grade sign-off metadata on every line and an industry overlay that adds the specialty steps a given client actually needs. Designed as the upstream hand-off to **financial-narrative-builder** (the advisory layer that consumes close outputs).

## When to Use

Use this skill at the start of a new bookkeeping or CAS engagement to establish the close process, when onboarding a new staff member to a client, when a client's business has changed (new entity, new state, new revenue streams, new lender covenant), or whenever you want to systematize and document the monthly close procedure for a specific client.

## Required Input

Provide the following:

1. **Client details** — Business name, entity type (sole prop, LLC, S-corp, C-corp, partnership, nonprofit), and industry (NAICS if known).
2. **Accounting software** — QuickBooks Online, QuickBooks Desktop, Xero, Sage Intacct, NetSuite, Sage 50, FreshBooks, or other.
3. **Accounting basis** — Cash, accrual, tax-basis, or modified cash.
4. **Key account types present** — Which of these apply: cash / bank accounts, accounts receivable, inventory, fixed assets, prepaid expenses, accounts payable, credit cards, loans / notes payable, payroll, sales tax, intercompany accounts, deferred revenue, multi-currency accounts.
5. **Revenue model** — How the client earns revenue (product sales, service billing, subscription / SaaS, project-based, mixed, commission).
6. **Payroll** — In-house, outsourced (to whom — Gusto, ADP, Paychex, Rippling), or no employees.
7. **State(s) of operation** — For sales tax and payroll tax reconciliation steps. Note any state with an income-tax PTE-election workaround.
8. **Special considerations** — Multi-entity consolidation, foreign currency transactions, grant reporting, construction WIP, restaurant tip liability, SaaS deferred-revenue waterfall, trust accounting, CAM / rent reconciliation (real estate), nonprofit restricted-fund tracking, inventory standard-cost variances (manufacturing).
9. **Covenant or lender-reporting constraints** — Any monthly or quarterly covenant calculation that must run off the closed financials (DSCR, fixed-charge coverage, min-liquidity, max leverage). Drives the covenant-tie-out step.

## Instructions

You are a skilled accounting professional's AI assistant. Your job is to produce a thorough, dependency-sequenced, workpaper-grade month-end close checklist that a bookkeeper or staff accountant can follow step by step to close the books accurately and completely, and that a reviewer / approver can sign off on without re-doing the work.

**Before you start:**
- Load `config.yml` from the repo root — pull firm name, target close cadence (e.g., **WD5 / WD7 / WD10** business-day close), materiality defaults (reclass threshold, post-JE threshold, investigate-variance threshold), and the firm's preparer / reviewer / approver routing. If the client has a client-specific close-cadence override, use that.
- Reference `knowledge-base/terminology/` for correct industry terms and the firm's tick-mark legend.
- Reference `knowledge-base/best-practices/` for the firm's workpaper documentation standard. If none is present, default to AICPA SSARS AR-C §70 / §80 / §90 documentation practice where an attest or preparation engagement is in scope.
- Use the firm's communication tone from `config.yml` → `voice`.

**Dependency ordering (hard prerequisites):**
- Bank reconciliations complete **before** revenue / AR review (cash receipts have to be posted first).
- Credit-card reconciliations complete **before** AP / expense review.
- Payroll posting complete **before** payroll-tax reconciliation.
- Inventory count posted **before** COGS / gross-margin review.
- All subsidiary-ledger reconciliations complete **before** balance-sheet account reconciliations.
- All reconciliations complete **before** journal entries and trial-balance review.

**Process — build the checklist in the following dependency-ordered sections. Include only sections relevant to the client's account types, entity, and industry.**

1. **Pre-close preparation**
   - Confirm all bank and credit card transactions are downloaded / imported through month-end.
   - Verify all client-provided documents are received: bank statements, credit card statements, loan statements, payroll reports, merchant processor statements, any third-party schedules (inventory count, WIP, tip log, etc.).
   - Confirm payroll has been posted for the month.
   - Confirm the prior month is locked in the accounting software and no post-close adjustments have been made.

2. **Bank and credit card reconciliation**
   - Reconcile each bank account to the statement balance.
   - Reconcile each credit card to the statement balance.
   - Investigate and resolve outstanding reconciling items older than 30 days.
   - Verify zero-balance accounts remain at zero; flag any unexpected activity.

3. **Revenue and receivables** *(requires bank rec complete)*
   - Review and post all revenue entries for the period.
   - Review AR aging — flag balances over 60 / 90 / 120 days; confirm bad-debt reserve is adequate.
   - For accrual-basis: post unbilled revenue accruals; reverse prior-month accruals.
   - Sales-return and refund reserve rolled where applicable.

4. **Expenses and payables** *(requires credit-card rec complete)*
   - Review AP aging for completeness.
   - For accrual-basis: accrue known expenses not yet billed (utilities, professional services, merchant fees, etc.); reverse prior-month accruals.
   - Verify payroll expense reconciles to payroll reports (gross wages, employer taxes, benefits, PTO accrual).
   - Reconcile sales tax collected to sales tax payable; confirm returns filed by jurisdiction.

5. **Balance sheet account reconciliations** *(requires subsidiary ledgers reconciled)*
   - Inventory: reconcile to count or adjust per physical count (if applicable); post std-cost / FIFO / LIFO variances.
   - Prepaids: amortize insurance, rent, software, and other prepaid balances; post prepaid-rollforward.
   - Fixed assets: record additions, disposals, and run monthly depreciation; reconcile to fixed-asset subledger.
   - Loans: reconcile balances to lender statements; bifurcate principal vs. interest expense; verify amortization per loan schedule.
   - Intercompany: reconcile intercompany balances (must net to zero at consolidation).
   - Deferred revenue: recognize the earned portion for the period; confirm waterfall ties to contract terms.
   - Accrued liabilities: reconcile each accrued-liability account to supporting schedule.

6. **Payroll tax reconciliation** *(requires payroll posted)*
   - Verify 941 / 940 liability balances match payroll-provider reports.
   - Confirm state withholding and SUI / SDI deposits are current.
   - Reconcile payroll tax liability accounts.

7. **Sales tax reconciliation**
   - Confirm sales tax collected matches sales tax payable account by jurisdiction.
   - Verify filings are current for each jurisdiction; note any economic-nexus threshold crossings.

8. **Journal entries and adjustments** *(requires all recs complete)*
   - Post all recurring journal entries (depreciation, amortization, allocations).
   - Post non-recurring adjustments with supporting documentation.
   - Review prior-period adjustments — ensure prior-month entries were not altered.

9. **Industry overlay (only the subset that applies to the client)**
   - **Construction / Contractor** — Post WIP schedule; reconcile over/under billings; roll backlog; verify job-cost postings by phase code.
   - **Restaurant / Hospitality** — Reconcile tip liability; post meal / entertainment allocation; reconcile merchant-processor net-of-fees deposits; run daily-sales-summary ties to POS.
   - **SaaS / Subscription** — Roll deferred-revenue waterfall; post contract-modification adjustments per ASC 606; reconcile ARR and billings to GL revenue.
   - **Real Estate / Property Management** — CAM reconciliation; tenant escrow; trust-account three-way reconciliation (bank / book / tenant ledger).
   - **Nonprofit** — Restricted / unrestricted net-asset rollforward; grant-reporting tie-out; donor-restriction release JEs.
   - **Professional Services** — WIP-to-billings reconciliation; utilization and realization ties; unbilled-revenue rollforward.
   - **Manufacturing** — Standard cost variance analysis (purchase price, usage, rate, efficiency); scrap and rework adjustments; perpetual-to-physical inventory tie.
   - **Trust / IOLTA (legal, escrow agents)** — Three-way reconciliation (bank / book / client-ledger); matter-level balance confirmation; bar-rule compliance sign-off.

10. **Covenant and lender-reporting tie-out** *(only if covenants exist)*
    - Pull the covenant-calculation worksheet.
    - Tie each covenant input to the closed financials.
    - Compute covenant result and compare to threshold; compute headroom.
    - Flag any breach or <5%-headroom result for immediate partner / client notice.

11. **Review and quality control**
    - Run trial balance and scan for unusual balances (negative assets, negative revenue, debit-balance liabilities).
    - Compare key line items to prior month, prior year, and budget — investigate variances exceeding **10% AND $5,000** (or the firm / client override).
    - Review unclassified / "Ask My Accountant" / suspense transactions — reclassify all.
    - Verify retained earnings / equity / members' capital rollforward is clean.

12. **Close the period**
    - Set the close date in the accounting software to prevent changes.
    - Export or save final trial balance, financial statements, and supporting workpaper pack.
    - Note carryover items for next month (reversing accruals, prepaid amortizations still running, open reconciling items).

13. **Advisory hand-off (to financial-narrative-builder)**
    - Package closed TB + CY / PY / budget P&L + balance sheet + AR aging + AP aging + KPI snapshot and pass to the financial-narrative-builder skill. This is what converts the close from a compliance output into the advisory deliverable.

**Output requirements:**
- Numbered checklist with checkboxes, grouped by the sections above, in the dependency order shown. Do not reorder sections so that a downstream step is listed before its prerequisite.
- Skip sections not relevant to the client (e.g., no inventory for a service business, no intercompany for a single entity, no industry-overlay items outside the client's industry).
- Each step should be specific enough that a staff accountant knows exactly what to do, and should carry a workpaper-sign-off block: **Preparer / Preparer Date / Reviewer / Reviewer Date / Tick-Mark / Supporting Document Ref**.
- Include a **"Documents Needed from Client"** summary at the top, with a deadline pegged to the firm's close-cadence target (e.g., "Due by WD3 for a WD7 close").
- Include a **close-cadence header**: target close day, responsible preparer, reviewer, approver (all from `config.yml`); estimated hours per section based on client complexity.
- Include a final **partner / manager sign-off line** — close is not complete until signed off.
- Professional formatting suitable for a recurring monthly procedure document that will be reused for months / years.
- Save to `outputs/close-checklists/{ClientSlug}-monthend.md`.

## Example Output

> [This section will be populated by the eval system with a reference example. For now, run the skill against a construction client on QBO with a WD7 close target, a covenant-bearing term loan, and one restricted-fund-equivalent retainage account, and verify that (a) dependency ordering is preserved, (b) the construction WIP overlay appears and the SaaS / restaurant overlays do not, (c) every reconciliation step carries the preparer / reviewer sign-off block, (d) the covenant tie-out section appears and references the specific covenants, and (e) the advisory hand-off block names the financial-narrative-builder skill as the downstream consumer.]
