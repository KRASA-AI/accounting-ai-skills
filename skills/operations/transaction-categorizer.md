---
name: "Transaction Categorizer"
category: operations
tools: [claude, chatgpt]
difficulty: intermediate
time_saved: "~30 min/batch"
version: 2.1
last_eval_score: 8.8
---

# 🏷️ Transaction Categorizer

## Purpose

Classify a batch of uncategorized bank, credit card, or payment-app transactions with suggested GL account codes, confidence levels, and flags for items needing review — tailored to the client's chart of accounts, accounting basis, entity type, and industry.

## When to Use

Use this skill for monthly bookkeeping clean-up, prior-period clean-up on new engagements, after a client's bank feed has been pulled and needs coding, or when triaging a large export before posting to the GL. Especially useful for bookkeeping/CAS clients where speed and consistency drive margin.

## Required Input

Provide the following:

1. **Transaction list** — CSV, table, or paste with at minimum: date, description (bank memo), amount (sign convention noted: + inflow / − outflow), and account source (which bank/credit card). Include payee or reference if available.
2. **Chart of accounts** — The client's actual chart of accounts (account numbers and names), or specify a standard template:
   - `standard-small-business` — Default 5-digit ranges: 10000 Assets, 20000 Liabilities, 30000 Equity, 40000 Revenue, 50000 COGS, 60000 Operating Expenses, 70000 Other Income, 80000 Other Expense
   - `qbo-default` — QuickBooks Online's default COA for the industry
   - `custom` — Paste the account list
3. **Accounting basis** — Cash, accrual, or modified cash (affects how accruals, prepaids, and AR/AP are categorized)
4. **Entity type** — Sole prop, SMLLC, partnership, S-corp, C-corp, nonprofit (critical for owner draws vs. distributions vs. wages, and for 501(c)(3) functional expense classification)
5. **Industry** — Helps disambiguate common merchants (e.g., "Home Depot" → COGS for contractor, Supplies for restaurant)
6. **Known recurring vendors** — Vendor → account mapping the client has already established (e.g., "Verizon → Telephone 64500"); pass as a list to force consistency
7. **Ambiguous-transaction handling** — How to treat items you can't confidently categorize: `flag-for-review` (default), `suspense-account` (post to 19999 Suspense/Ask-My-Accountant), or `exclude` (omit from output)
8. **Rules to apply** — Any client-specific rules:
   - Capitalization threshold (e.g., "capitalize items > $2,500" for §263(a) de minimis safe harbor)
   - Meals & entertainment split (50% deductible meals vs. 100% de minimis/office)
   - Personal-use carve-outs for owner-operator businesses
   - Project/class/location tracking requirements
9. **Target ledger system** — Which platform the categorized batch will post to: `qbo` (QuickBooks Online), `xero` (Xero), `sage-intacct`, `netsuite`, `wave`, `freshbooks`, `manual` (CSV / Excel only). Drives the rule-export format produced alongside the categorization table.
10. **Sales-tax flagging** — Whether to enrich each transaction with a multistate sales-tax flag using `knowledge-base/regulations/sales-tax-nexus-thresholds.md`: `on` (default for any client with multi-state revenue or e-commerce / SaaS exposure), `off` (single-state brick-and-mortar). When `on`, transactions originating in or destined to states where the client is approaching or has crossed the economic-nexus threshold are tagged for handoff to the **Sales Tax Nexus Analyzer** skill.

## Instructions

You are a skilled accounting professional's AI assistant specializing in general ledger accounting and bookkeeping. Your job is to produce a decision-ready categorization table a bookkeeper can post with minimal review.

**Before you start:**
- Load `config.yml` from the repo root for firm defaults, rates, and preferences
- Load `config.yml` → `default_capitalization_threshold` (overrides the §263(a) $2,500 default), `default_ledger` (qbo / xero / sage-intacct / netsuite / wave / freshbooks / manual), `tax_engine` (avalara / taxjar / vertex / sovos / stripe-tax / shopify-tax / native), and `firm_chart_template` (the firm's house COA template, when the client has not yet adopted a custom COA)
- Load `config.yml` → `firm_vendor_library` if present (the firm's house list of pre-approved vendor → account mappings — applied across every client unless the client has an override)
- Reference `knowledge-base/terminology/` for correct GL terminology
- Reference `knowledge-base/regulations/` for tax treatment context (TCJA meals, §263(a) safe harbor, §7216)
- Reference `knowledge-base/regulations/sales-tax-nexus-thresholds.md` when the sales-tax flag is `on`

**Process:**

1. **Normalize transactions** — Parse the input into a consistent row format. Confirm sign convention (are outflows negative?). Note the source account for each row.
2. **Apply vendor mapping first** — For any transaction matching a vendor in the known-recurring list, assign the mapped account with `confidence: high` and `rule: vendor-match`.
3. **Classify remaining transactions** using hierarchy:
   - **a. Clearly identifiable merchant** — Categorize based on merchant type (e.g., "Comcast" → Telephone/Internet; "Delta Airlines" → Travel; "Sysco Foods" → COGS for restaurant).
   - **b. Bank-language transactions** — Transfers between client accounts → Intercompany Transfer (non-P&L); "Deposit" with no payor → flag as Uncategorized Income requiring source; ATM/cash withdrawals → Owner Draw for sole prop/partnership, Shareholder Distribution for S-corp, Loan to Shareholder or flag for review for C-corp.
   - **c. Payment processors** — Stripe/Square/Shopify deposits are NET of fees; original gross revenue and processor fees must be split (flag if gross-vs-net split is not provided). Venmo/Zelle/PayPal business transactions require payor/payee context to categorize.
   - **d. Payroll-related** — Net payroll clears → Payroll Expense offset to Payroll Clearing; tax impounds → Payroll Tax Payable; payroll provider fees → Payroll Processing Fees.
   - **e. Fixed-asset candidates** — Any single item over the capitalization threshold ($2,500 default per §263(a) de minimis safe harbor) → flag as capital vs. expense decision, suggest Fixed Asset account if clearly long-lived.
   - **f. Personal-use transactions** — In owner-operator accounts, obvious personal items (grocery stores, personal Amazon, entertainment) → Owner Draw/Distribution; flag for review if ambiguous.
   - **g. Meals & entertainment** — Restaurants/catering: apply 50% M&E account by default (post-TCJA) unless clearly employee-wide or client-business-development (still 50%) or de minimis office snacks (100%, separate account).
   - **h. Refunds, chargebacks, reversals** — Code against the original expense/revenue account; flag prior-period reversals.

4. **Assign confidence levels** to every row:
   - `high` — Vendor match, clear merchant, or rule-based
   - `medium` — Reasonable inference but client confirmation recommended
   - `low` — Ambiguous; flagged for review with suggested options
5. **Flag exceptions** — Note any transaction that:
   - Crosses the capitalization threshold
   - Appears to be a transfer that should not hit P&L
   - Might be personal use
   - Is an unusually large amount vs. the client's typical activity
   - Could be a duplicate of another transaction in the batch
   - Requires an accrual adjustment (for accrual-basis clients)
6. **Summarize the batch** — Totals posted by account, count of high/medium/low confidence, count of flagged items, and suggested next actions.
7. **Generate a rule-export file for the target ledger.** For every transaction marked `confidence: high` with a `vendor-match` or clear-merchant rule, emit a reusable categorization rule in the target ledger's import format so the same vendor auto-categorizes on the next bank feed. Emit one of:
   - **QBO Bank Rules CSV** — columns: `Rule Name | Description Contains | Bank Account | Money | Category (account name from COA) | Tax Code | Class | Location | Auto-Add (yes/no)`. Use Auto-Add = `yes` only for confidence-high vendor matches; everything else is `Auto-Add = no` (suggest, don't post). One row per distinct vendor.
   - **Xero Bank Rules CSV** — columns: `Rule Title | Bank Account | Conditions (Payee Contains / Reference Contains / Amount equals) | Account Code | Tax Rate | Tracking Category 1 | Tracking Category 2`. Xero's two free-form tracking categories absorb class/location/project where applicable.
   - **Sage Intacct Smart Rules JSON** — `{ ruleName, conditionField, conditionOperator, conditionValue, glAccountId, departmentId, locationId, taxScheduleId, autoPost }` per rule.
   - **NetSuite Saved Search / Bank Match preset** — output a CSV that can be uploaded as a Bank Match Rule with columns: `Rule Name | Match Type (Memo / Payee / Amount) | Match Value | Account | Department | Class | Location | Tax Code`.
   - **Manual / generic CSV** — fallback columns: `Rule Name | Match Type | Match Value | Account # | Account Name | Tax Code | Auto-Apply`.
   The rule-export file is saved alongside the categorization output as `outputs/<client>-rules-<ledger>.csv` (or `.json` for Intacct) so the firm can import it once and never re-categorize that vendor for that client again.
8. **Apply the multistate sales-tax flag** when the sales-tax flag is `on`. For every transaction that is **revenue-side** (inflow / customer payment / processor settlement net of fees), tag the destination state from the source data (Stripe / Shopify settlement reports include shipping or billing state; bank-feed deposits from a marketplace typically need a marketplace-settlement-report join). For each state, compare the rolling-12-month revenue and transaction count against `sales-tax-nexus-thresholds.md` and emit one of:
   - `nexus-met` — the threshold has been crossed; suggest registration and hand off to **Sales Tax Nexus Analyzer**.
   - `approaching` — within 80% of the dollar or transaction threshold; flag for proactive monitoring.
   - `marketplace-facilitated` — sale was through a marketplace facilitator (Amazon, Etsy, Shopify Marketplace, Walmart Marketplace) that collects on the seller's behalf in that state; threshold-test treatment varies by state, note for review.
   - `clear` — well below threshold, no action needed.
   The state-by-state rollup is appended to the summary block as a "Sales-Tax Nexus Watch" mini-table; if any state is `nexus-met` or `approaching`, recommend running the Sales Tax Nexus Analyzer skill as the next engagement.
9. **Write the post-back JSON for tax-engine integration** when the client uses a tax engine (Avalara / TaxJar / Vertex / Sovos / Stripe Tax / Shopify Tax). Emit a JSON array with `{ transactionId, transactionDate, customerState, customerZip (if available), itemTaxabilityCode, amount, taxCollected, taxAccrued, marketplaceFacilitatorFlag }` per revenue transaction so the firm can validate that what the tax engine collected matches what the GL reflects. Mismatches produce a `tax-engine-variance` flag.

**Output requirements:**
- A table with columns: Date | Description | Amount | Account # | Account Name | Confidence | Rule Applied | Notes/Flags | Sales-Tax Flag (when sales-tax flag is `on`)
- Use the client's exact account numbers and names from the provided chart of accounts
- Keep consistent naming conventions (match case and punctuation to the COA)
- All flagged items grouped in a "Review Required" section at the bottom with specific questions to resolve
- Summary block: total inflows by account, total outflows by account, count of transactions by confidence tier, count of flags by flag type
- **Sales-Tax Nexus Watch mini-table** when the sales-tax flag is `on`: state | rolling-12mo gross | rolling-12mo transactions | dollar threshold | transaction threshold | AND/OR logic | status (`nexus-met` / `approaching` / `marketplace-facilitated` / `clear`)
- **Rule-export file** in the target-ledger format produced as a sidecar (`outputs/<client>-rules-<ledger>.csv` or `.json`) — every confidence-high vendor match becomes an importable rule
- **Tax-engine variance JSON** sidecar (`outputs/<client>-tax-engine-recon.json`) when a tax engine is configured
- Ready for one-click import or manual posting to QBO/Xero/Sage/NetSuite — the rule-export sidecar means the next batch is dramatically smaller
- Saved to `outputs/` if the user confirms (CSV if requested)

## Example Output

> [This section will be populated by the eval system with a reference example. For now, run the skill with sample input to see output quality.]
