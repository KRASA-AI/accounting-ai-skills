---
name: "Transaction Categorizer"
category: operations
tools: [claude, chatgpt]
difficulty: intermediate
time_saved: "~30 min/batch"
version: 2.2
last_eval_score: 9.0
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
- Load `config.yml` → **13 named pulls**: `firm_partner` (the partner of record), `firm_name`, `firm_bookkeeping_lead` (the CAS / bookkeeping team lead), `default_capitalization_threshold` (overrides the §263(a) $2,500 default), `default_ledger` (qbo / xero / xero-os / xero-jax / sage-intacct / netsuite / wave / freshbooks / ies / ias / ies-construction / manual), `tax_engine` (avalara / taxjar / vertex / sovos / stripe-tax / shopify-tax / native), `firm_chart_template` (the firm's house COA template, when the client has not yet adopted a custom COA), `firm_vendor_library` (the firm's house list of pre-approved vendor → account mappings — applied across every client unless the client has an override), `default_meals_split_policy` (50% TCJA default vs. 100% de minimis carve-out vs. firm-specific override), `default_personal_use_handling` (auto-flag vs. auto-post-to-owner-draw vs. exclude-from-output for owner-operator entities), `client_segment_routing` (per-segment SLA — bookkeeping clients get a different review cadence than audit-prep clients), `peer_benchmark_source` (mirrors variance-analyzer's row — BizMiner / RMA / IBISWorld / Sageworks / Intuit Enterprise Suite / Xero JAX / firm-house — drives the cross-skill handoff to Variance Analyzer when peer-benchmarked spend variance is flagged), `aiuc1_disclosure_default` (on / off — whether the AIUC-1 conditional citation block fires on AI-tool-categorized batches by default)
- Reference `knowledge-base/terminology/` for correct GL terminology
- Reference `knowledge-base/regulations/` for tax treatment context (TCJA meals, §263(a) safe harbor, §7216)
- Reference `knowledge-base/regulations/sales-tax-nexus-thresholds.md` when the sales-tax flag is `on`

**Ledger-Backbone Selection (resolve `default_ledger` from config.yml; each backbone drives auto-categorization-rule shape, job-cost / dimensional posting expectations, sales-tax-engine bridge, peer-benchmark availability, and the rule-export sidecar format):**

| Backbone | Segment | Auto-categorization shape | Dimensional posting | Sales-tax bridge | Peer-benchmark | Rule-export sidecar |
|---|---|---|---|---|---|---|
| **Intuit Enterprise Suite (IES)** | Mid-market (May 2026 IES expansion) | IES agentic categorization with peer-benchmarked variance from millions of IES baselines; agent runs as a sub-agent of the firm's IAS workflow | Multi-entity + class + location + project (4 dimensions); IES enforces dimension on every COGS / direct-labor / fixed-asset line | Native IES sales-tax module; bridges to Avalara / Vertex via marketplace connectors | Yes — IES peer-benchmark median (auto-flagged variance) | `outputs/<client>-rules-ies.json` (IES agentic-rule shape) |
| **Intuit Accountant Suite (IAS)** | Firm-side partner platform (the QBOA successor; firms manage QBO / IES / IES-Construction client books from inside IAS) | Firm-tier rule library shared across client subset; the firm's `firm_vendor_library` posts here once and applies to every IAS-managed client | Inherits the underlying client-book backbone (QBO / IES / IES-Construction) | Inherits client-book backbone | Inherits client-book backbone | `outputs/<client>-rules-ias-{client-backbone}.json` |
| **IES Construction Edition** | Construction (Intuit's first industry-specific ERP) | Job-cost-aware agentic categorization with WIP-equity reconciliation built in; auto-routes direct-labor + materials + sub-cost + equipment time to job-cost dimension | Job + phase + cost-code + class (4 construction-specific dimensions); WIP/over-billings reconciliation runs nightly | Native IES sales-tax + state contractor-license tax bridge | Yes — IES Construction Edition peer-benchmark median (overrides generic Construction row) | `outputs/<client>-rules-ies-construction.json` |
| **Xero OS** | Mid-market (the mid-market backbone successor / sibling to standard Xero; multi-entity consolidation) | Xero OS auto-categorization with multi-entity rule inheritance from parent org | Multi-entity + 2 free-form tracking categories (class / location / project) | Native Xero sales-tax module; bridges to Avalara via app marketplace | Yes — Xero OS peer-benchmark (mid-market) | `outputs/<client>-rules-xero-os.csv` |
| **Xero JAX** | Small / mid-market (Xero's auto-categorization + cash-flow triage agent) | JAX vendor-pattern recognition with auto-categorization confidence scoring; JAX flags low-confidence rows for human review | 2 free-form tracking categories | Xero sales-tax + JAX cash-flow triage | Yes — Xero JAX peer-benchmark (small / mid-market) | `outputs/<client>-rules-xero.csv` (standard Xero Bank Rules format; JAX overlays auto-apply) |
| **QuickBooks Online (QBO)** | Small business (standard) | QBO Bank Rules + Intuit Assist suggestions | Class + location (2 dimensions) | QBO native sales-tax + Avalara / TaxJar app | No (defer to BizMiner / RMA / Sageworks if peer-benchmark called for) | `outputs/<client>-rules-qbo.csv` (QBO Bank Rules CSV format) |
| **Sage Intacct** | Mid-market (multi-entity / multi-dimensional) | Sage Intacct Smart Rules + Intacct Copilot suggestions | Multi-entity + up to 8 dimensions (department / location / project / customer / vendor / item / class / employee) | Sage Intacct AvaTax / Vertex / Sovos connectors | No (defer to BizMiner / RMA / Sageworks) | `outputs/<client>-rules-intacct.json` |
| **NetSuite** | Upper-mid-market / enterprise | NetSuite Bank Match Rules + Oracle Joule / NS AI suggestions | Multi-subsidiary + class + department + location + custom segments | NetSuite SuiteTax + Avalara / Vertex / Sovos connectors | No (defer to BizMiner / RMA / Sageworks) | `outputs/<client>-rules-netsuite.csv` |

If `default_ledger = ies-construction`, force the Construction-vertical override on every fixed-asset, COGS, and direct-labor line: post to job-cost dimension before posting to GL. If `default_ledger = ias`, inherit the underlying client-book backbone row; the rule-export sidecar names the backbone in the filename.

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

10. **Cross-skill handoff** (partner-facing addendum — separate from the categorization table the bookkeeper posts). Route based on what surfaced in the batch:
    - Any transaction flagged `nexus-met` or `approaching` (from step 8) → **Sales Tax Nexus Analyzer** (10-row Marketplace-Facilitator Treatment Overlay flagged; pre-load the rolling-12-month state-by-state mini-table as the input)
    - Construction-vertical WIP / over-billings reconciliation variance (only when `default_ledger = ies-construction` or industry = construction) → **Variance Analyzer** (Industry-Overlay Variance Lens Construction row + Peer-Benchmark Backbone Selection — cite `peer_benchmark_source` from config)
    - R&D-coded transactions (§174A wage buckets / §41 supply or contract-research lines) → **R&D Credit Documenter** (8-vertical Industry R&D Profile Overlay flagged; pre-load the categorized R&D rows as the project-time / supply / contract-research input)
    - Payroll-tax variance (941 / state-UC variance, owner-comp vs. distribution mis-coding for S-corp) → **Compliance Tracker** (federal information-return calendar + state payroll-tax row flagged)
    - Fixed-asset capitalization decision over threshold (§263(a) de minimis + §168(k) bonus depreciation 100% restoration TY2025+) → **Tax Memo Writer** (OBBBA Provision Quick-Lookup Overlay flagged — §168(k) row, plus §263A UNICAP for inventory-heavy clients)
    - Peer-benchmarked spend variance flagged by IES / Xero JAX / Sageworks → **Variance Analyzer** (cite peer-benchmark source row)
    - Month-end close approaching after categorization batch posts → **Month-End Checklist**
    - First-time bookkeeping engagement onboard → **Client Onboarding Package** (Bookkeeping / CAS column of the Service-Type × Entity-Type Document Matrix flagged) + **Engagement Letter Generator** (Bookkeeping / CAS profile + AI-Tool Disclosure & Reliance Clause Library row flagged)
    - Suspicious-pattern flag (round-dollar transfers, unusual vendor, off-hours posting) on a PCAOB-issuer or other significant-risk engagement → **Fraud Risk Brainstorm** (12-vertical Industry Fraud-Scenario Library overlay flagged)
    - Going-concern flag (consistent expense-over-revenue + working-capital decline) → **Going Concern Assessment** (12-vertical Industry Trigger Library overlay flagged)
    - Client-facing communication required on a recurring categorization rule change or material adjustment → **Client Email Drafter** (22-row Regulatory / Deadline Calendar Overlay row flagged)
    - IRS / state notice arrives on a categorization-flagged item → **IRS Notice Responder** (10-authority State Notice Overlay flagged)
    The handoff block is a partner-facing addendum unless `client_segment_routing` explicitly authorizes client delivery (most segments do not).

**AIUC-1 conditional citation block** (fires when `aiuc1_disclosure_default = on` or when any AI / agentic tool participated in categorization): when categorization was assisted by an AI or agentic tool — Intuit Enterprise Suite agentic categorization, Intuit Assist (QBO), Xero JAX auto-categorization, Sage Intacct Copilot, NetSuite Oracle Joule, Vic.ai or Booke.ai categorization layer, FloQast Visual Agent Builder reconciliation, Suralink Workpaper Suite Intelligence source-link extraction, or a native-LLM rule-export generator — the partner-facing addendum cites the tool stack and its certification posture: **AIUC-1** (the AI-tool assurance standard; Schellman is the first authorized AIUC-1 certifier as of 2026) alongside **SOC 2 Type II** and **ISO/IEC 42001**. Output package row: "AI-tool reliance — tools: {list}; AIUC-1 status: {certified / in-process / not pursuing} per `aiuc1_disclosure_default` and the firm's tool-stack inventory; human-reviewer retains professional responsibility per AICPA Rule 102 / Rule 201 (the AI tool is not the preparer of record)." For PCAOB-registered audit work, also reference **PCAOB AS 1215** (Dec 15, 2026) electronic-working-paper / AI-tool documentation requirements.

**Output requirements:**
- A table with columns: Date | Description | Amount | Account # | Account Name | Confidence | Rule Applied | Notes/Flags | Sales-Tax Flag (when sales-tax flag is `on`)
- Use the client's exact account numbers and names from the provided chart of accounts
- Keep consistent naming conventions (match case and punctuation to the COA)
- All flagged items grouped in a "Review Required" section at the bottom with specific questions to resolve
- Summary block: total inflows by account, total outflows by account, count of transactions by confidence tier, count of flags by flag type
- **Sales-Tax Nexus Watch mini-table** when the sales-tax flag is `on`: state | rolling-12mo gross | rolling-12mo transactions | dollar threshold | transaction threshold | AND/OR logic | status (`nexus-met` / `approaching` / `marketplace-facilitated` / `clear`)
- **Rule-export file** in the target-ledger format produced as a sidecar (`outputs/<client>-rules-<ledger>.csv` or `.json`) — every confidence-high vendor match becomes an importable rule
- **Tax-engine variance JSON** sidecar (`outputs/<client>-tax-engine-recon.json`) when a tax engine is configured
- **Cross-skill handoff addendum** (`outputs/<client>-handoff.md`) — partner-facing routing of every flagged item from step 10 (Sales Tax Nexus Analyzer / Variance Analyzer / R&D Credit Documenter / Compliance Tracker / Tax Memo Writer / Month-End Checklist / Client Onboarding Package / Engagement Letter Generator / Fraud Risk Brainstorm / Going Concern Assessment / Client Email Drafter / IRS Notice Responder)
- **AIUC-1 tool-stack disclosure row** when `aiuc1_disclosure_default = on` or any AI / agentic tool participated — tools, AIUC-1 / SOC 2 Type II / ISO/IEC 42001 certification posture, human-reviewer professional-responsibility statement
- Ready for one-click import or manual posting to QBO/Xero/Xero OS/Sage/NetSuite/IES/IES Construction/IAS — the rule-export sidecar means the next batch is dramatically smaller
- Saved to `outputs/` if the user confirms (CSV if requested)

## Example Output

> [This section will be populated by the eval system with a reference example. For now, run the skill with sample input to see output quality.]
