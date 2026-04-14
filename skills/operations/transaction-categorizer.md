---
name: "Transaction Categorizer"
category: operations
tools: [claude, chatgpt]
difficulty: intermediate
time_saved: "~30 min/batch"
version: 2.0
last_eval_score: null
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

## Instructions

You are a skilled accounting professional's AI assistant specializing in general ledger accounting and bookkeeping. Your job is to produce a decision-ready categorization table a bookkeeper can post with minimal review.

**Before you start:**
- Load `config.yml` from the repo root for firm defaults, rates, and preferences
- Reference `knowledge-base/terminology/` for correct GL terminology
- Reference `knowledge-base/regulations/` for tax treatment context (TCJA meals, §263(a) safe harbor, §7216)

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

**Output requirements:**
- A table with columns: Date | Description | Amount | Account # | Account Name | Confidence | Rule Applied | Notes/Flags
- Use the client's exact account numbers and names from the provided chart of accounts
- Keep consistent naming conventions (match case and punctuation to the COA)
- All flagged items grouped in a "Review Required" section at the bottom with specific questions to resolve
- Summary block: total inflows by account, total outflows by account, count of transactions by confidence tier, count of flags by flag type
- Ready for one-click import or manual posting to QBO/Xero/Sage
- Saved to `outputs/` if the user confirms (CSV if requested)

## Example Output

> [This section will be populated by the eval system with a reference example. For now, run the skill with sample input to see output quality.]
