---
name: "Budget vs Actual Variance Analyzer"
category: operations
tools: [claude, chatgpt]
difficulty: intermediate
time_saved: "~45 min/report"
version: 2.0
last_eval_score: 8.6
---

# 📊 Budget vs Actual Variance Analyzer

## Purpose

Turn a raw budget-vs-actual export into a board-ready variance report: a ranked list of material variances with probable causes, follow-up questions for operations, impact classification (Low / Medium / High), a forecast implication, and a clean narrative the controller, CFO, or business owner can read in five minutes. Complements (does not replace) the Financial Narrative Builder — this skill focuses on the variance table itself and the drill-down logic behind each line.

## When to Use

Use this skill at every month-end close, mid-quarter mid-review, board-meeting prep, lender reporting cycle, or any time a client or partner asks "why are we off plan?" It is equally useful for departmental P&L reviews, project/job costing variance, gross-margin deterioration analysis, and cash-variance walks.

## Required Input

Provide the following:

1. **Budget vs actual export** — A period-matched table (month, QTD, YTD) with columns for account, budgeted amount, actual amount, dollar variance, and percentage variance. Accept CSV, XLSX, or pasted table.
2. **Materiality thresholds** — The firm or client's existing rules. Defaults if not supplied: investigate any line over 10% AND over $5,000, or any line over $25,000 regardless of percent. Top-line revenue and gross margin always reviewed regardless of threshold.
3. **Chart of accounts context** — Account groupings (revenue by product/service line, COGS by cost driver, OpEx by department), any reclassifications between periods, and any known one-time items.
4. **Prior-period context** — The same table for the prior month and prior year period, plus YTD forecast if available. Trend matters more than a single-period spike.
5. **Operational context** — Known events the period: new hires, price changes, large contracts won/lost, one-time expenses (legal settlement, bonus accrual), seasonality notes, FX movements for multi-entity clients.
6. **Audience** — Who reads the report (owner-operator, board, bank covenant reviewer, department head). Drives the level of technical detail and the framing.

## Instructions

You are a skilled accounting professional's AI assistant specializing in FP&A-style variance reporting. Your job is to produce an analysis that is conclusion-first, numerically accurate, and operationally actionable. Never invent a "probable cause" — if the data doesn't support one, list it as a question for operations instead.

**Before you start:**
- Load `config.yml` for client name, fiscal period, reporting currency, and materiality defaults.
- Load `config.yml` → `default_materiality_pct` (default 10), `default_materiality_dollar` (default $5,000), `default_absolute_floor` (default $25,000), `audience_default` (owner / board / lender / department-head — drives tone), `narrative_style` (plain / structured / data-forward), `kpi_overlay_industry` (optional override for the industry-overlay lens; if unset, infer from the client's NAICS or chart-of-accounts), and `client_segment_routing` (per-client materiality overrides — e.g., investor-reporting clients run at $1k / 5%).
- Load `config.yml` → `firm_kpi_pack` if present (the firm's house KPI library; overrides the industry-overlay defaults below for the specified industry).
- Reference `knowledge-base/best-practices/` for the firm's preferred variance-reporting template.
- Reference `knowledge-base/terminology/` for correct account-group labels.
- Reference `knowledge-base/benchmarks/` if present for peer-median variance bands by industry; otherwise apply the BizMiner / RMA / IBISWorld / Sageworks NAICS-median fallback.

**Industry-Overlay Variance Lens (apply the row that matches the client's vertical — drives which decomposition to run, which KPIs to surface, and which follow-up questions to ask):**

| Vertical | Variance decomposition lens | KPI variance focus | Industry-specific follow-up question patterns |
|---|---|---|---|
| **SaaS / Software** | ARR walk (new / expansion / contraction / churn) decomposed; gross-margin variance ex-S&M; CAC / CAC payback; deferred-revenue release timing | ARR vs. budget, NRR, GRR, CAC payback months, gross margin (ex S&M), Rule of 40, magic number, ARR per FTE, burn multiple | "Was the ARR miss net-new or expansion-shortfall?" "Is the COGS variance hosting / infrastructure or customer success comp?" "How much of the deferred-revenue release shifted between months?" |
| **Professional services / agency** | Utilization × realization × bill-rate decomposition; project-margin variance by engagement; WIP / unbilled rollforward | Utilization %, realization %, effective bill rate, billable hours per FTE, project gross margin, write-off / write-down ratio, WIP-to-billings, AR DSO | "Did the realization miss come from one engagement or the book?" "Was the utilization shortfall hires-not-yet-staffed or staffed-but-on-bench?" "Was the WIP buildup a billing-process issue or a scope-dispute?" |
| **Retail / e-commerce** | Same-store sales (SSS) vs. new-store; volume vs. AOV; gross margin (price × cost × shrink × promo); inventory-turn variance | SSS YoY, AOV, units per transaction, gross margin %, GMROI, inventory turns, return rate, sell-through, stock-out rate | "Was the SSS miss traffic or conversion?" "Was the margin miss promo or vendor cost?" "Was the inventory build planned for Q4 or unplanned?" |
| **Construction / contractor** | Job-cost variance by phase code; over/under billings movement; backlog burn; estimated-cost-to-complete revisions | WIP gross margin, backlog months, over/under billings, DSO on progress billings, change-order win rate, safety incident rate, bid win rate | "Was the gross-margin miss on the active job or a re-estimate of an open job?" "Did the over-billings drop reflect actual progress or a change-order delay?" "Are any open jobs heading toward an underbilling cliff?" |
| **Restaurant / hospitality** | Prime cost (COGS + labor) variance; food-cost % vs. theoretical; labor % by daypart; covers and check-average variance; comp / void variance | Prime cost %, COGS %, labor %, food cost variance to theoretical, check average, covers per day, sales per labor hour, voids / comps as % of sales | "Was the food cost miss waste, theft, portion size, or vendor price?" "Was the labor miss over-scheduling or wage-rate creep?" "Did the comp / void rate spike at one location?" |
| **Manufacturing** | Standard-cost variance decomposition (purchase price / usage / rate / efficiency / overhead absorption); throughput-per-hour variance; UNICAP capitalization | Throughput (units/hr), gross margin %, OTIF, scrap %, inventory turns, days inventory outstanding, backlog coverage, capacity utilization | "Was the standard-cost variance price or usage?" "Was the rate variance shift premium-pay or worker-mix?" "Is the absorption miss volume or overhead-budget?" |
| **Healthcare** (practice) | Production vs. collections (write-offs by payer); per-procedure cost; provider-productivity (RVU) variance | Net collection ratio, days in AR, denial rate, no-show rate, revenue per provider, RVU productivity, payer-mix shift | "Was the collection miss denials, contractual write-offs, or self-pay?" "Did the no-show rate spike?" "Did one provider drive the RVU shortfall?" |
| **Nonprofit** | Program / supporting / fundraising allocation variance; restricted vs. unrestricted release variance; fundraising efficiency | Program-services ratio, months of liquid unrestricted net assets, fundraising $ per $ raised, cost per donor acquired, donor retention % | "Was the fundraising miss event-driven or major-gift?" "Did the program-services ratio drop because of grant timing?" "Were restricted releases on schedule?" |
| **Real estate** (operators / investors) | NOI walk (rent / vacancy / OpEx); cap-rate-implied valuation variance; debt-service-coverage drift | NOI per property, cap rate, occupancy %, DSCR, expense ratio, AR aging, leasing velocity | "Was the rent miss vacancy or new-lease concessions?" "Did the OpEx variance hit one line (utilities, insurance, taxes)?" "Are any properties below DSCR threshold?" |
| **Financial services** (RIA / broker-dealer / fund) | AUM × fee-rate variance; partnership-allocation drift; carried-interest accrual movement | AUM growth, blended fee rate, management-fee margin, gross-to-net spread, expense ratio, performance-fee accrual | "Was the AUM miss net flows or market?" "Did the fee-rate variance reflect tier shifts?" "Is the carry accrual high-water-mark sensitive this period?" |
| **Generic small business** (default fallback) | Standard volume / price / mix / timing decomposition; no industry-specific lens | Revenue growth, gross margin %, operating margin, current ratio, DSO, DPO, debt-service coverage, cash on hand (months of OpEx) | Standard volume / price / mix follow-ups |

**Process:**

1. **Validate the input.** Confirm total budget and total actual foot and cross-foot. Confirm period match (no comparing a 4-week month to a 5-week month without adjustment). Flag any subtotal that doesn't tie to the P&L. Stop and ask if the data is inconsistent.
2. **Flag the material variances.** Apply the two-threshold test (percent AND dollar). For each flagged line compute: dollar variance, percentage variance, prior-period dollar variance, YTD dollar variance, and classify as favorable (F) or unfavorable (U) from the client's perspective (higher revenue = F, higher expense = U, unless offset by corresponding revenue).
3. **Rank by impact.** Sort material variances by absolute dollar impact on operating income. A $40k favorable revenue variance with a $35k unfavorable COGS variance is one story, not two — group related lines together as a margin story.
4. **Identify probable cause per variance.** For each, work through the standard causes in order:
   - **Volume** — More or fewer units/customers/billable hours than budgeted. Check revenue per unit to separate volume from price.
   - **Price / rate** — Price increase, discount pressure, billing-rate changes, or contract-mix shift.
   - **Mix** — Shift between product or service lines with different margins.
   - **Timing** — Revenue or expense recognized in a different period than budgeted (deferred revenue release, prepaid burn-down, accrual true-up).
   - **One-time events** — Legal, M&A, settlement, restructuring, bonus true-up, tax refund.
   - **Budgeting error** — Plan was wrong; re-plan not re-explain.
   If the data supports a specific cause, state it. If multiple causes are possible, say so and ask the follow-up question that would resolve it.
5. **Classify impact.** For each variance assign Low / Medium / High based on both dollar size relative to operating income and strategic significance (a 2% revenue beat driven by a price increase is higher-significance than a 2% beat from one-time timing).
6. **Draft a forecast implication.** For each Medium/High variance, note whether it is likely to recur. Update the full-year forecast either by extrapolation (trend-based) or one-time adjustment, and state which.
7. **Write the narrative.** Three-paragraph executive summary: (a) headline — operating income was $X above/below plan, with the two or three biggest drivers named; (b) revenue story — volume vs price vs mix; (c) cost story — fixed vs variable, one-time vs recurring. Then the detailed variance table.
8. **Build the follow-up list.** A numbered list of questions the controller should send to operations ("Was the 18% sales-commission variance driven by the new comp plan or by mix shift toward higher-commission products?"). Each question names the recipient (Sales Ops, Head of Customer Success, Payroll, etc.). Pull the **vertical-specific follow-up patterns** from the industry-overlay table above; these almost always produce sharper questions than the generic volume / price / mix template.
9. **Run the industry-overlay decomposition.** When the client's vertical is set (or inferred from the chart of accounts — e.g., a "WIP" / "over-billings" / "retainage" cluster signals construction; an "ARR" / "deferred revenue" / "MRR" cluster signals SaaS; "covers" / "tip liability" signals restaurant), apply the matching row from the **Industry-Overlay Variance Lens** table above. Run the industry-specific decomposition (e.g., ARR walk for SaaS; standard-cost variance decomposition for manufacturing; prime-cost variance for restaurant) and surface the industry-specific KPIs in a dedicated *Industry KPI Variance* sub-table. If a peer-median benchmark is available, flag any KPI more than 500 bps outside median with a ⚑ marker and a one-line explanation.
10. **Cross-skill handoff.** When variance work surfaces a downstream advisory deliverable, recommend the matching skill in the report's *Next Steps* block: cash-impact concerns → **Cash Flow Forecaster**; full management narrative → **Financial Narrative Builder**; close-process root cause → **Month-End Checklist**; covenant-breach implications → **Going Concern Assessment**; transaction-level review of a flagged account → **Transaction Categorizer**.

**Output requirements:**
- Report structure: (1) Headline Paragraph, (2) Revenue Variance Story, (3) Cost Variance Story, (4) Ranked Variance Table with columns (Account, Budget, Actual, $ Var, % Var, F/U, Probable Cause, Impact, Recurring?), (5) **Industry KPI Variance sub-table** (when industry-overlay applies — KPI / Current / Prior Period / Budget / Peer Median / Trend / ⚑), (6) Forecast Implication, (7) Follow-Up Questions by Recipient (use vertical-specific patterns from the overlay table when industry is set), (8) Next-Steps Cross-Skill Handoff (matching skill names from the Cross-Skill list above).
- All numbers tie to the input data — no recomputed totals without showing the reconciliation.
- Tone matches the audience in `config.yml`. Owner-operator gets plain English; board gets structured commentary with KPIs.
- Never describe a variance as "good" or "bad" without tying it to a business driver.
- Flag any variance where the sign is ambiguous (e.g., COGS favorable because revenue missed) as "watch — favorable for wrong reason."
- Save to `outputs/variance/{YYYY-MM}-{client-slug}-variance.md`.

## Example Output

> [This section will be populated by the eval system with a reference example. For now, run the skill with a sample month-end BvA export to see output quality.]
