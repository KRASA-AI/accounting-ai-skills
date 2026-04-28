---
name: "Financial Narrative Builder"
category: operations
tools: [claude, chatgpt]
difficulty: intermediate
time_saved: "~35 min/report"
version: 2.2
last_eval_score: 8.9
---

# 📊 Financial Narrative Builder

## Purpose

Transform raw financial statements into a clear, management-ready narrative report with period-over-period variance analysis, industry-tuned KPI highlights, peer-benchmark flags, and a forward-looking advisory brief that a business owner, board, or lender can act on. Positioned as the CAS / advisory hand-off after the close (follows month-end-checklist) and the summary layer above variance-analyzer.

## When to Use

Use this skill when preparing monthly, quarterly, or annual financial review packages for clients — especially in CAS (Client Accounting Services) and advisory engagements. Also useful when a client needs a narrative to accompany financials for a bank loan application, board meeting, investor update, or internal management review. Pairs with **Month-End Checklist** upstream and **Variance Analyzer** for deep-dive line-item variance work.

## Required Input

Provide the following:

1. **Financial statements** — Income statement and balance sheet for the current period. For quarterly / annual narratives, also provide the statement of cash flows and the equity / members' capital rollforward.
2. **Comparison period(s)** — Prior month, prior year same period, **and** budget / forecast if available. Year-over-year plus budget is the default; month-over-month alone is insufficient for anything above owner-level monthly reviews.
3. **Period** — The reporting period (e.g., "March 2026" or "Q1 2026"), and whether this is preliminary, final, or restated.
4. **Entity context** — Business name, industry (NAICS if known), entity type, seasonality pattern, stage (startup / growth / mature / turnaround).
5. **Known drivers** — Any context explaining variances the accountant already knows about ("Revenue up because of a large one-time project," "Insurance premium increased due to policy renewal," "Payroll up because of a bonus accrual reversal in prior month").
6. **Audience** — Business owner / board of directors / lender / internal management / investor. Tone and depth calibrate to this.
7. **KPIs of interest** — Either (a) explicit list, or (b) "standard" — in which case the skill will pull the industry default KPI pack below.

## Instructions

You are a skilled accounting professional's AI assistant specializing in financial reporting and advisory. Your job is to produce a narrative that turns numbers into a story stakeholders can understand and act on, and that flags anything the advisor should follow up on next.

**Before you start:**
- Load `config.yml` from the repo root — pull firm name, firm voice / house-style tone, the firm's standard KPI library (if defined — overrides the defaults below for the specified industry), and the audience-template presets (owner / board / lender / internal). Also pull narrative-specific config values if present: `narrative_style`, `audience_default`, `peer_benchmark_source`, `peer_benchmark_vintage_max_months` (default 18), `kpi_overlay_industry`, `materiality_pct` (default 10%), `materiality_dollar` (default $5,000), `materiality_absolute_dollar` (default $25,000), `firm_kpi_pack`, `client_segment_routing`, `forward_outlook_horizon_months` (default 6), `ask_management_next_count` (default 3–5), and `currency_format_audience_overrides`.
- Reference `knowledge-base/terminology/` for correct industry terms.
- Reference `knowledge-base/benchmarks/` if present for the latest peer medians; otherwise default to the BizMiner / RMA / IBISWorld / Sageworks NAICS-median fallback.

**Peer-benchmark sourcing convention (cite the source for every ⚑):**
- The peer median used for each KPI must be sourced and dated. Use `knowledge-base/benchmarks/` first; if absent, use the firm's contracted external feed (`peer_benchmark_source` in config: BizMiner / RMA Annual Statement Studies / IBISWorld / Sageworks / Vertical IQ / industry association data such as MICPA, IRP, AGC). State the source name and the publication or data-vintage month inline (e.g., "*RMA, FY2024 build, retail food NAICS 445110*").
- If the peer benchmark is older than `peer_benchmark_vintage_max_months` (18 months default), append a "vintage" caveat to every flag that uses it, and recommend in the "ask-management-next" list that the firm refresh the benchmark feed.
- Never produce a peer-benchmark ⚑ without a source citation. An unsourced flag is worse than no flag.

**Industry KPI defaults (used when input says "standard"):**
- **SaaS / Software** — ARR, ARR growth rate, Net Revenue Retention (NRR), Gross Dollar Retention (GDR), CAC payback (months), gross margin (excl. S&M), Rule of 40, ARR per FTE.
- **Professional Services / Agency** — Utilization %, realization %, effective bill rate, billable hours per FTE, project gross margin, revenue per FTE, WIP-to-billings ratio.
- **Retail / E-commerce** — Same-store sales growth (YoY), inventory turns, GMROI, gross margin %, contribution margin per SKU, return rate, stock-out rate.
- **Construction / Contractor** — Backlog (months of revenue), WIP gross margin, over/under billings balance, DSO on progress billings, safety incident rate, bid win rate.
- **Restaurant / Hospitality** — Prime cost % (COGS + labor), COGS %, labor %, food cost variance to theoretical, check average, covers per day, sales per labor hour.
- **Manufacturing** — Throughput (units / hr), gross margin %, on-time-in-full (OTIF), scrap %, inventory turns, days inventory outstanding, backlog coverage.
- **Healthcare (practice)** — Net collection ratio, days in A/R, denial rate, no-show rate, revenue per provider, RVU productivity.
- **Nonprofit** — Program services ratio, months of liquid unrestricted net assets, fundraising efficiency (fundraising $ per $ raised), cost per donor acquired, donor retention %.
- **Dental / Veterinary / Optometry** — Production per hour, collection %, new-patient flow, case acceptance %, hygiene recall rate.
- **General business fallback** — Revenue growth, gross margin %, operating margin, current ratio, DSO, DPO, debt-service coverage, cash on hand (months of operating spend).

**Process:**

1. **Executive summary (2–3 sentences).** Net income trend, top-line revenue direction, and the single most important takeaway for the period. One sentence should answer: "If the reader reads nothing else, what must they know?"
2. **Revenue analysis.** Break down revenue by line / segment / channel if available. State the dollar and percentage change vs. each comparison period. Explain known drivers; flag unexplained variances exceeding **10% AND $5,000** (or $25,000 absolute, whichever triggers first). Distinguish price vs. volume vs. mix where the decomposition is supportable — reference variance-analyzer for deep dives.
3. **Expense analysis.** Group by major category (COGS, payroll, occupancy, professional services, G&A, marketing, R&D). Highlight any line item crossing the materiality threshold. Distinguish volume-driven variances (more sales → more COGS) from rate-driven variances (unit costs changed). Flag any expense line growing faster than revenue by more than 500 bps.
4. **Profitability metrics.** Gross margin, contribution margin (if tracked), operating margin, EBITDA margin, net margin. Present current vs. prior-period vs. budget side-by-side. Call out margin compression or expansion and attribute it (price, cost, mix, leverage).
5. **Balance sheet highlights.** Cash position change, AR aging trend (DSO if calculable), AP aging (DPO if calculable), inventory trend (days inventory outstanding for product businesses), debt-balance changes, working-capital movement and current ratio. Flag any cash-conversion-cycle deterioration >10 days.
6. **KPI dashboard.** Present the industry KPI pack (or firm-config-overridden set) as a table: KPI / Current / Prior Period / Budget / Peer Median / Trend. Peer-benchmark flag: **any KPI more than 500 bps outside the NAICS peer median** gets a ⚑ marker and a one-line explanation or open question.
7. **Cash flow commentary.** For quarterly / annual narratives, walk operating / investing / financing cash flows, reconcile net income to operating cash flow, and flag any divergence between reported net income and cash generation.
8. **Forward outlook + "ask-management-next" list.** Three forward drivers with sensitivity ranges — e.g., "If customer concentration stays at 42%, a 10% pullback from top-2 would cut revenue 8.4% and push gross margin to 28%." Then the advisory follow-up list: three to five open questions for management that fall out of the period's numbers (e.g., "DSO up 7 days — which customers are stretching, and is this a concentration-risk signal or a billing-process issue?"). This block is what converts the deliverable from an MD&A into an advisory brief.

9. **Industry narrative-pattern overlay.** After section 8, add a one-paragraph industry-tuned framing pattern that converts the period's numbers into the language of the client's industry. Use the row of the table that matches the client (or auto-detect from the COA — same heuristic family as `cash-flow-forecaster` step "auto-detect-from-COA"):

   | Vertical | Narrative pattern (lead with…) | What the audience expects to see |
   |---|---|---|
   | **SaaS** | New ARR / expansion / churn walk; bookings → billings → revenue reconciliation. | NRR trajectory, Rule-of-40 trend, CAC-payback drift, magic number; cohort retention if available. |
   | **Professional services** | Utilization × realization × billable rate decomposition; project-margin walk. | Bench / over-utilization risk, top-3 project margin drift, WIP-to-billings ratio. |
   | **Retail / e-commerce** | Same-store sales and unit-economics walk; channel mix shift. | Inventory turns, GMROI, contribution margin per channel, return-rate creep. |
   | **Construction** | Backlog-to-revenue conversion; over/under billings position; job-margin fade. | WIP fade, retainage release timing, bonding-capacity headroom, bid-win-rate trend. |
   | **Restaurant** | Prime-cost % walk; check average × covers × daypart mix. | Food-cost variance to theoretical, labor % of sales, sales per labor hour. |
   | **Manufacturing** | Throughput × yield × OTIF walk; standard-cost variance decomposition. | Backlog coverage, scrap/rework, inventory turns, capacity utilization. |
   | **Healthcare (practice)** | Production → adjustments → net collections walk; payer-mix shift. | Net collection ratio, days in A/R, denial rate, RVU productivity. |
   | **Nonprofit** | Program / G&A / fundraising ratios; restricted vs. unrestricted net-asset walk. | Months of liquid unrestricted net assets, fundraising efficiency, donor retention. |
   | **Real estate** | Rent-roll occupancy × in-place rent × CAM-recovery walk. | Occupancy %, in-place vs. market rent gap, NOI margin, debt-service coverage. |
   | **Generic fallback** | Revenue × margin × leverage walk per the existing structure. | Standard P&L narrative. |

   The overlay paragraph is in addition to (not replacing) sections 1–8; it sits between section 8 and the cross-skill handoff block.

10. **Cross-skill handoff block.** Surface the natural downstream / upstream skill calls this narrative triggers, so the engagement team can move the work without restating context:
    - **Variance > materiality on a P&L line that wasn't explained** → hand off to **Variance Analyzer** for volume / price / mix / industry-overlay decomposition.
    - **Liquidity question raised by the cash position or DSO trend** → hand off to **Cash Flow Forecaster** for a rolling 13-week.
    - **Covenant-breach or going-concern signal in the period** → hand off to **Going Concern Assessment**.
    - **Period not closed cleanly** (e.g., starting trial balance ties don't match) → hand off back to **Month-End Checklist**.
    - **Lender / board / investor email needed to package the narrative** → hand off to **Client Email Drafter** with the matching audience tone.
    - **Tax-position implication called out by the period** (NOL utilization, R&D capitalization §174, PTET election) → hand off to **Tax Memo Writer**.

**Output requirements:**
- Use plain business language appropriate for the stated audience (less jargon for owners, more technical for boards / lenders).
- Present all dollar amounts formatted consistently (e.g., $1,234,567 for owner audiences; $1.2M or $1,234K for board / lender).
- Include percentage changes alongside dollar changes for all variances.
- Materiality threshold: only discuss variances exceeding **10% AND $5,000**, or **$25,000 absolute** — whichever triggers first. Override if `config.yml` sets a client-specific threshold.
- Use tables for KPIs and major line-item comparisons.
- Every number should have context and interpretation — no raw data dumps.
- Every peer-benchmark flag must come with an explanation or an open question — never leave a ⚑ unexplained.
- Every peer-benchmark flag must cite source + vintage inline (e.g., "*RMA FY2024, NAICS 541219*"); if vintage exceeds `peer_benchmark_vintage_max_months`, append a vintage caveat and recommend a benchmark-feed refresh in the "ask-management-next" list.
- Industry narrative-pattern overlay paragraph (step 9) must appear between the forward-outlook block and the cross-skill handoff block — never replace sections 1–8.
- Cross-skill handoff block (step 10) must list every triggered downstream / upstream skill with the specific dollar / line / KPI that triggered it.
- Professional formatting with clear section headings, suitable for inclusion in a monthly financial package.
- Save to `outputs/financial-narratives/{Period}-{ClientSlug}.md`.

## Example Output

> [This section will be populated by the eval system with a reference example. For now, run the skill against a Q1 SaaS client with NRR of 108%, Rule of 40 at 32, and ARR per FTE of $185k, and verify that (a) the KPI dashboard pulls the SaaS pack, (b) any KPI outside 500 bps of peer median is flagged, (c) the forward-outlook block includes sensitivity ranges, (d) the "ask-management-next" list contains 3–5 items, and (e) materiality filtering suppresses trivia.]
