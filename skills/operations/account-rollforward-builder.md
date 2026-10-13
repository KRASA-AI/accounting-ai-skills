---
name: "Balance Sheet Account Rollforward Builder"
category: operations
tools: [claude, chatgpt]
difficulty: intermediate
time_saved: "~40 min/account"
version: 1.1
last_eval_score: 9.0
---

# 🔁 Balance Sheet Account Rollforward Builder

## Purpose

Build a GL-tied rollforward workpaper for one balance-sheet account (or a defined account group) for one period: opening balance → categorized activity → closing balance, with every line traced to a ledger query or source document, a mechanical foot check, and any difference left visible as an **unreconciled residual** rather than absorbed into a line. The result is a close-package schedule a reviewer can sign off on and an auditor can request as PBC support without rework.

Complements **Month-End Checklist** (which tells you *which* rollforwards the close requires) and **Variance Analyzer** (which explains P&L movement). This skill produces the balance-sheet schedule itself.

## When to Use

- Month-, quarter-, or year-end close for accounts that move through activity rather than a single balance confirmation: prepaids, accrued liabilities, deferred revenue, fixed assets and accumulated depreciation, notes payable, allowance for doubtful accounts, reserves (returns, warranty, chargebacks), gift-card/loyalty liabilities, equity / capital accounts, intercompany balances.
- Building PBC support for the auditor's request list ("provide rollforwards for PP&E, debt, and accrued liabilities").
- Re-performing a client-prepared rollforward that does not tie to the trial balance.
- Investigating why an account's closing balance moved more than expected.

## Required Input

1. **Account(s) and entity** — GL account number(s) and name(s), entity, reporting currency, and the period (start and end dates).
2. **Opening balance source** — the prior-period signed-off rollforward / close package, or the GL / trial balance at prior period-end. If both are available, provide both (they will be compared).
3. **Activity detail for the period** — GL detail export (date, JE number, journal source / type, description, debit, credit) for the account(s). CSV, XLSX, or pasted table.
4. **Closing balance source** — the period-end trial balance.
5. **Supporting schedules (if any)** — amortization schedules, debt amortization tables, fixed-asset subledger, deferred-revenue waterfall, reserve calculation, FX rates used.
6. **Category map (optional)** — the firm's or client's standard rollforward line categories for this account type. If omitted, the defaults in the Instructions are used.

## Instructions

You are a senior accountant's AI assistant preparing a balance-sheet rollforward workpaper. Your output will be reviewed and may be relied on by an auditor, so every number must trace to an input and nothing may be forced to tie.

**Before you start:**
- Load `config.yml` → `firm_name`, `default_engagement_team.firm_controller_lead` (preparer of record if none given), `preparer_reviewer_routing` (drives the sign-off block), `default_absolute_floor` and `clearly_trivial_threshold_pct` (drive how the unreconciled residual is classified — see step 6), `default_capitalization_threshold` (used for the fixed-asset additions check), `je_testing_tool` (named in the manual-entry flag so the reviewer knows where to follow up), and `industry_overlay_pack` (selects any extra industry rollforward lines — e.g., construction retainage, SaaS deferred-revenue / commission asset, nonprofit restricted net assets).
- Reference `knowledge-base/best-practices/ai-generated-workpaper-standards.md`: classification decisions are required inputs, not silent defaults (convention #1); derived figures are recomputed before delivery (convention #2); spreadsheet companions keep inputs and formulas visually distinct (convention #3).

**Default activity categories by account type** (use the client's category map instead when supplied):

| Account type | Activity lines (in order) |
|---|---|
| Prepaid expenses | Additions (payments) · Amortization to expense · Reclasses · FX |
| Accrued liabilities | New accruals · Reversals of prior accruals · Payments / settlements · True-ups · Reclasses |
| Deferred revenue / contract liabilities | Billings · Revenue recognized · Contract modifications · Refunds / credits · Reclasses |
| Fixed assets (cost) | Additions (capitalized) · Disposals / retirements · Transfers from CIP · Impairments · FX |
| Accumulated depreciation | Depreciation expense · Disposals · Impairments · FX |
| Notes / loans payable | Draws / new borrowings · Principal repayments · Amortization of issuance costs / discount · Reclass current ↔ long-term · FX |
| Allowances & reserves | Provision charged · Write-offs / utilization · Recoveries · Remeasurement |
| Equity / capital | Contributions · Distributions / dividends · Net income allocation · Other comprehensive income · Stock comp / other |
| Intercompany | Charges out · Charges in · Settlements · FX — counterparty balance must be shown alongside |

**Process:**

1. **Establish the opening balance.** Take it from the prior signed-off rollforward. If you also have the GL balance at prior period-end, compare them; if they differ, stop and report the difference as a **prior-period tie-out exception** (likely a post-close entry to a closed period) before going further. Never silently pick one.
2. **Categorize the activity.** Assign every GL detail line to exactly one activity category using journal source, description, and offsetting account. Lines you cannot categorize with confidence go to an explicit *Unclassified — needs preparer decision* line; do not guess (convention #1). Report the count and dollar total of unclassified lines.
3. **Document the trace for each line.** For each category, record how it ties: the GL filter used (account, date range, journal source), the JE numbers included, and/or the supporting schedule and its total. A category with no trace is incomplete, not zero.
4. **Establish the closing balance** from the period-end trial balance.
5. **Foot and tie.** Compute Opening + Σ activity = Calculated closing, and compare it to the TB closing balance. Also confirm the GL detail total equals the net TB movement (detail completeness). Recompute every subtotal yourself; do not accept input subtotals unchecked (convention #2).
6. **Handle any difference honestly.** If Calculated closing ≠ TB closing, show the gap as a separate **Unreconciled residual** line. Never adjust or plug an activity line to make the schedule tie. Classify the residual:
   - below `clearly_trivial_threshold_pct` of performance materiality → *Clearly trivial — document and pass*;
   - below `default_absolute_floor` → *Investigate before sign-off*;
   - at or above `default_absolute_floor` → *Blocking — do not mark the rollforward complete*.
   For each residual, list the most likely causes to check (cutoff, entry posted to wrong account, FX rate mismatch, subledger-to-GL timing, manual top-side entry).
7. **Run reasonableness checks** appropriate to the account type, for example: amortization / depreciation expense vs. the supporting schedule and vs. prior period run-rate; fixed-asset additions below `default_capitalization_threshold` (possible expense items capitalized); accruals reversed but not re-accrued; deferred revenue recognized faster than the contract terms imply; debt principal vs. lender statement; current/long-term reclass present at period end; intercompany balance mirrors the counterparty.
8. **Flag manual and unusual entries.** List manual JEs, entries posted after period-end, round-dollar entries, and entries by unusual preparers or with blank descriptions, so the reviewer can follow up in `je_testing_tool`.
9. **Cross-skill handoff.** Flagged classification questions on individual transactions → **Transaction Categorizer**; P&L impact of a reserve or accrual true-up → **Variance Analyzer**; missing close step discovered → **Month-End Checklist**; debt covenant or liquidity concerns → **Going Concern Assessment** / **Cash Flow Forecaster**; audit coverage mapping of this schedule to assertions → **Audit Coverage Matrix**.

**Output requirements:**

- **Header:** client, entity, account(s), period, currency, preparer, reviewer, date, and source files used.
- **Rollforward table:** columns *Line · Amount · Ties to (GL query / JE range / schedule) · Notes*. Rows: Opening balance → each activity category → Unclassified (if any) → Calculated closing → TB closing → **Unreconciled residual** → **Foot check: PASS / FAIL**.
- **Prior-period tie-out** result (step 1) and **detail-completeness** result (step 5).
- **Exceptions and reviewer flags:** unreconciled residual with its classification and likely causes; unclassified lines; reasonableness-check findings; manual / unusual entries.
- **Sign-off block** that follows `preparer_reviewer_routing`; leave reviewer fields blank for the human reviewer. Status is *Complete* only if the foot check passes and no blocking residual or unclassified line remains; otherwise *Open — items listed above*.
- **Spreadsheet companion (optional):** when XLSX output is available, write closing balance and residual as live formulas over the input cells, with inputs and formulas visually distinguished; confirm there are no error cells before presenting.
- Save to `outputs/rollforwards/{YYYY-MM}-{client-slug}-{account-slug}-rollforward.md`.
- Every number ties to an input or a shown calculation. Never state that the account "reconciles" when the residual is non-zero.

## Example Output

*Illustrative only — fictional client, figures chosen to show an open item.*

> **Rollforward — Prepaid Insurance (GL 1410)** · Client: Northwind Fabrication LLC · Period: Sep 1–30, 2026 · USD
> Preparer: Joan Whitfield · Reviewer: ________ · Sources: Aug-2026 signed close package; Sep GL detail export; Sep 30 TB; insurance amortization schedule v3
>
> | Line | Amount | Ties to | Notes |
> |---|---:|---|---|
> | Opening balance | 24,000.00 | Aug-2026 close package, GL 1410 | Prior-period tie-out: close package = GL at Aug 31 ✔ |
> | Additions (payments) | 18,000.00 | JE 2026-09-004 (AP source) | Umbrella policy renewal, 12-mo term from Sep 1 |
> | Amortization to expense | (5,500.00) | JE 2026-09-118 (recurring); schedule v3 Sep total | GL policy $4,000 + umbrella $1,500 |
> | Unclassified — needs preparer decision | (150.00) | JE 2026-09-131 (manual, blank description) | Offset to 6420 Office Supplies |
> | **Calculated closing** | **36,350.00** | | |
> | TB closing | 36,350.00 | Sep 30 TB | |
> | **Unreconciled residual** | **0.00** | | |
> | **Foot check** | **PASS** | | Detail completeness: GL detail net 12,350.00 = TB movement 12,350.00 ✔ |
>
> **Exceptions and reviewer flags**
> 1. *Unclassified line:* JE 2026-09-131 credits prepaid insurance and debits office supplies with no description — likely a mis-coded entry. Ask the preparer to support or reverse; review in MindBridge.
> 2. *Reasonableness:* amortization of $5,500 is up $1,500 from the August run-rate. The increase matches the new umbrella policy (18,000 ÷ 12). No exception.
>
> **Status: Open** — the schedule foots, but one unclassified line is still unresolved. Routing: Staff prepares → Manager reviews → Partner signs.
