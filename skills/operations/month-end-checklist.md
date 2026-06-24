---
name: "Month-End Checklist"
category: operations
tools: [claude, chatgpt]
difficulty: intermediate
time_saved: "~25 min/client"
version: 2.3
last_eval_score: 9.0
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
- Load `config.yml` from the repo root — pull firm name, target close cadence (e.g., **WD5 / WD7 / WD10** business-day close), materiality defaults (reclass threshold, post-JE threshold, investigate-variance threshold), and the firm's preparer / reviewer / approver routing. If the client has a client-specific close-cadence override, use that. Pull these close-specific config values if present: `default_close_cadence` (WD5 / WD7 / WD10), `client_close_cadence_override`, `materiality_reclass_threshold`, `materiality_post_je_threshold`, `materiality_investigate_threshold`, `tick_mark_legend`, `preparer_reviewer_routing`, `partner_signoff_required` (boolean), `industry_overlay_pack`, `entity_type_overlay_pack`, `service_tier` (CAS-Lite / CAS-Standard / CAS-Plus / Outsourced-Controller), `wisp_path`, and `lender_reporting_pack`.
- Reference `knowledge-base/terminology/` for correct industry terms and the firm's tick-mark legend.
- Reference `knowledge-base/best-practices/` for the firm's workpaper documentation standard. If none is present, default to AICPA SSARS AR-C §70 / §80 / §90 documentation practice where an attest or preparation engagement is in scope.
- Use the firm's communication tone from `config.yml` → `voice`.

**Entity-type overlay (apply the row that matches the client's entity; cumulative with the industry overlay):**

| Entity type | Equity / capital reconciliation steps | Distribution / draw mechanics | Tax-step adjustments |
|---|---|---|---|
| **Sole proprietor** | Owner's-equity rollforward; reconcile owner's-draw account to bank evidence. | Track owner draws separately from business expenses (don't sweep into G&A). | Schedule SE-tax estimate accrual; flag commingled personal expenses for reclass. |
| **Single-member LLC (disregarded)** | Same as sole prop unless §761 / check-the-box election made. | Member-draw reconciliation; distinguish from guaranteed payment if elected. | Same as sole prop. |
| **Multi-member LLC / Partnership** | §704(b) capital account rollforward by partner; reconcile distributions and contributions to operating-agreement waterfall. | Guaranteed payments accrued separately; distributions vs. allocation-of-profit segregation. | PTET election state-by-state; basis-tracking workpaper if engagement-scope; §163(j) interest-limitation flag. |
| **S-corp** | Reconcile AAA / OAA / E&P; distribute basis among shareholders for shareholder K-1; verify reasonable-comp wage. | Distributions reconciled to ownership %; flag any disproportionate distribution (single-class-of-stock violation). | Reasonable-comp accrual review; PTET election; §1366 / §1367 basis tracking; built-in gains tax (former C-corp). |
| **C-corp** | E&P rollforward; APIC and treasury-stock movements; tax-distribution policy if any. | Dividend declaration / payment cutover; shareholder loans flagged for §7872 imputed interest. | ASC 740 deferred-tax provision (handed to **Tax Provision Reviewer** when shipped); §163(j); CAMT trigger thresholds. |
| **Nonprofit / 501(c)** | Restricted vs. unrestricted net-asset rollforward; donor-restriction release JEs; functional-expense allocation. | N/A (no equity distributions); board-restricted reserves separate. | UBIT exposure flag; Form 990 program-services ratio; grant-deliverable tracking. |
| **Trust / Estate** | Principal / income segregation per UPIA; fiduciary accounting standards (NOT GAAP); 1041 DNI build. | Beneficiary distributions tracked separately for principal vs. income. | DNI tracking; throwback-rule flag for accumulated trusts; state composite return obligations. |
| **Multi-entity group** | Intercompany reconciliation must net to zero at consolidation; eliminations workpaper; minority-interest rollforward. | Intra-group dividends / distributions tracked but eliminated. | State combined / unitary filings; transfer-pricing documentation if applicable. |

**Dependency ordering (hard prerequisites):**
- Bank reconciliations complete **before** revenue / AR review (cash receipts have to be posted first).
- Credit-card reconciliations complete **before** AP / expense review.
- Payroll posting complete **before** payroll-tax reconciliation.
- Inventory count posted **before** COGS / gross-margin review.
- All subsidiary-ledger reconciliations complete **before** balance-sheet account reconciliations.
- All reconciliations complete **before** journal entries and trial-balance review.

**Step 0 — Pre-Flight Input Validation (run before building the checklist).** A close checklist that has to be re-issued because the entity type, software, or covenant status was unknown wastes the time the skill is meant to save. Resolve every gap in one pass:

1. **Check the nine Required Input items** (client details, software, basis, account types present, revenue model, payroll, states, special considerations, covenant constraints); mark each `present` / `missing` / `inferable`.
2. **Infer what the chart of accounts and software safely imply** before asking — a "deferred revenue" / "ARR" cluster implies the SaaS overlay; "WIP" / "retainage" implies construction; "tip liability" implies restaurant; a single legal entity implies no intercompany section; no inventory accounts implies skipping COGS/inventory steps. List every inference so the reviewer can correct it.
3. **Pull close-runtime config** (`default_close_cadence`, `service_tier`, `materiality_*` thresholds, `preparer_reviewer_routing`, `industry_overlay_pack`, `entity_type_overlay_pack`) so cadence, materiality, and sign-off routing are set without asking.
4. **Batch only the genuinely unresolved gaps into ONE numbered question list** — e.g., "Is this client accrual or cash basis?" and "Are there lender covenants that must tie to the closed financials?" — rather than discovering them mid-build.
5. **State the safe defaults you will assume if the user replies "proceed"**: accrual basis, the firm's `default_close_cadence` for the service tier, the 10%-AND-$5,000 variance investigate threshold, and no covenant tie-out unless covenants are named — each logged at the top of the checklist so a one-shot run is complete and auditable, while a user who wants to confirm first can.

The output of Step 0 is either a clean go-ahead or a single consolidated input checklist — never a checklist built on guessed entity/basis/covenant facts that has to be rebuilt.

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
   - **Construction / Contractor** — Post WIP schedule; reconcile over/under billings; roll backlog; verify job-cost postings by phase code; bonding-capacity headroom snapshot.
   - **Restaurant / Hospitality** — Reconcile tip liability; post meal / entertainment allocation; reconcile merchant-processor net-of-fees deposits; run daily-sales-summary ties to POS; gift-card liability runoff.
   - **SaaS / Subscription** — Roll deferred-revenue waterfall; post contract-modification adjustments per ASC 606; reconcile ARR and billings to GL revenue; commission asset rollforward per ASC 340-40.
   - **Real Estate / Property Management** — CAM reconciliation; tenant escrow; trust-account three-way reconciliation (bank / book / tenant ledger); rent-roll tie-out; tenant-improvement amortization.
   - **Nonprofit** — Restricted / unrestricted net-asset rollforward; grant-reporting tie-out; donor-restriction release JEs; functional-expense allocation per ASU 2016-14.
   - **Professional Services** — WIP-to-billings reconciliation; utilization and realization ties; unbilled-revenue rollforward; project-margin fade analysis.
   - **Manufacturing** — Standard cost variance analysis (purchase price, usage, rate, efficiency); scrap and rework adjustments; perpetual-to-physical inventory tie; LIFO reserve roll if applicable.
   - **Trust / IOLTA (legal, escrow agents)** — Three-way reconciliation (bank / book / client-ledger); matter-level balance confirmation; bar-rule compliance sign-off.
   - **Healthcare (practice)** — Patient AR reconciled net of denial reserve; claims-clearinghouse tie-out; RVU production reconciliation; payer-mix walk; HIPAA / FTC Safeguards alignment confirmation.
   - **Retail / E-commerce** — Inventory tie to perpetual + physical-count adjustment; gift-card / loyalty liability rollforward; marketplace-reserve (Amazon, Shopify, eBay) tie-out; chargeback / return reserve.
   - **Agriculture / Farm** — §175 soil/water conservation accrual; §1301 income averaging hooks; commodity-inventory mark; CCC-loan reconciliation; crop-insurance proceeds segregation.
   - **Financial services / lender / broker-dealer** — Customer-funds segregation (15c3-3 if applicable); loan loss reserve / CECL roll; covenant-compliance certificate; net-capital computation (broker-dealer).

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

14. **Cross-skill handoffs (beyond financial-narrative-builder).** Surface every other downstream skill that the close legitimately triggers, so the engagement team can move work without restating context:
    - **Variance > materiality on any P&L line** → hand off to **Variance Analyzer** for volume / price / mix / industry-overlay decomposition.
    - **Cash trend deteriorating, covenant headroom < 10%, or DSO > 10 days worse than prior period** → hand off to **Cash Flow Forecaster**.
    - **Going-concern indicator surfaced during close (negative cash trend, debt-service shortfall, recurring losses)** → hand off to **Going Concern Assessment** (AU-C 570).
    - **IRS / state notice surfaced during the close (e.g., unreconciled tax-deposit cluster, mismatched 941 / state withholding)** → hand off to **IRS Notice Responder**.
    - **Sales-tax-nexus threshold approached or crossed in any state** → hand off to **Sales Tax Nexus Analyzer**.
    - **R&D / §174 capitalization or §41 credit posture changed during the period** → hand off to **R&D Credit Documenter**.
    - **First-year close on a new client** (or scope change on an existing one) → hand off back to **Client Onboarding Package** to refresh the document-request matrix.

**Service-tier × close-cadence guidance (use to set the close-cadence header — pull `service_tier` from `config.yml`):**

| Service tier | Default close target | Reviewer level | Close pack contents | Advisory hand-off |
|---|---|---|---|---|
| **CAS-Lite (compliance only)** | WD15 | Senior staff | TB, P&L, BS | None (compliance scope) |
| **CAS-Standard** | WD10 | Manager | TB, P&L, BS, AR / AP aging, KPI snapshot | Financial Narrative Builder (monthly summary) |
| **CAS-Plus / Advisory** | WD7 | Manager + Partner spot-review | Standard pack + 13-week cash flow + variance analysis + KPI dashboard | Financial Narrative Builder + Cash Flow Forecaster + Variance Analyzer |
| **Outsourced controller / fractional CFO** | WD5 | Partner sign-off | All of the above + covenant pack + board-ready narrative | Full advisory chain + lender / board email packaging via Client Email Drafter |

**Output requirements:**
- Run **Step 0 — Pre-Flight Input Validation** first: return the single consolidated input checklist if entity / basis / covenant / overlay facts are missing and not safely inferable; otherwise proceed straight to the checklist with an assumptions log at the top, so no second round-trip is needed.
- Numbered checklist with checkboxes, grouped by the sections above, in the dependency order shown. Do not reorder sections so that a downstream step is listed before its prerequisite.
- Skip sections not relevant to the client (e.g., no inventory for a service business, no intercompany for a single entity, no industry-overlay items outside the client's industry).
- Each step should be specific enough that a staff accountant knows exactly what to do, and should carry a workpaper-sign-off block: **Preparer / Preparer Date / Reviewer / Reviewer Date / Tick-Mark / Supporting Document Ref**.
- Include a **"Documents Needed from Client"** summary at the top, with a deadline pegged to the firm's close-cadence target (e.g., "Due by WD3 for a WD7 close").
- Include a **close-cadence header**: target close day, responsible preparer, reviewer, approver (all from `config.yml`); estimated hours per section based on client complexity. Pull the close target from the service-tier table above unless `client_close_cadence_override` is set.
- Include the **entity-type overlay row** that fired (sole prop / SMLLC / partnership / S-corp / C-corp / nonprofit / trust / multi-entity), with the equity-rollforward, distribution, and tax-step adjustments inline so the preparer doesn't need to remember which apply.
- Include the **industry-overlay subset** that fired — and only that subset; never emit overlay steps for verticals the client doesn't operate in.
- Include the **cross-skill handoff block** (step 14) listing every triggered downstream skill with the specific dollar / line / KPI that triggered it.
- Include a final **partner / manager sign-off line** — close is not complete until signed off.
- Professional formatting suitable for a recurring monthly procedure document that will be reused for months / years.
- Save to `outputs/close-checklists/{ClientSlug}-monthend.md`.

## Example Output

> [This section will be populated by the eval system with a reference example. For now, run the skill against a construction client on QBO with a WD7 close target, a covenant-bearing term loan, and one restricted-fund-equivalent retainage account, and verify that (a) dependency ordering is preserved, (b) the construction WIP overlay appears and the SaaS / restaurant overlays do not, (c) every reconciliation step carries the preparer / reviewer sign-off block, (d) the covenant tie-out section appears and references the specific covenants, and (e) the advisory hand-off block names the financial-narrative-builder skill as the downstream consumer.]
