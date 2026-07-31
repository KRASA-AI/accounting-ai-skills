---
name: "Compliance Tracker"
category: admin
tools: [claude, chatgpt]
difficulty: intermediate
time_saved: "~20 min/review"
version: 2.2
last_eval_score: 9.0
---

# ✅ Compliance Tracker

## Purpose

Generate a comprehensive regulatory compliance calendar and checklist for a client based on their entity type, jurisdiction, industry, and active tax registrations. Tracks federal, state, and local filing deadlines, estimated payment due dates, annual report requirements, and industry-specific compliance obligations.

## When to Use

Use this skill at the start of a new engagement, at the beginning of each calendar or fiscal year, when a client expands to a new state or jurisdiction, or whenever you need to verify nothing is slipping through the cracks. Also useful for CAS and advisory engagements where proactive compliance monitoring is part of the service.

## Required Input

Provide the following:

1. **Entity type** — Sole proprietorship, single-member LLC, multi-member LLC, S-corp, C-corp, partnership, nonprofit, trust
2. **State(s) of operation** — All states where the client has nexus (physical presence, employees, or economic nexus)
3. **Industry** — Relevant for industry-specific filings (e.g., construction contractor licensing, healthcare HIPAA, restaurant health permits, financial services reporting)
4. **Fiscal year-end** — Calendar year or specific fiscal year-end date
5. **Payroll** — Whether the client has employees and in which states
6. **Sales tax** — Whether the client collects sales tax and in which jurisdictions
7. **Special registrations** — Any special licenses, permits, or registrations (e.g., excise tax, trust fund recovery, beneficial ownership reporting, state-level transparency-act filings, CAT / B&O / GET / IVU)
8. **Attest / assurance services in scope** — Whether the firm is performing compilation (SSARS §70), preparation, review (SSARS §90), audit (SAS), or any PCAOB-registered work for the client. Drives whether **PCAOB QC 1000** and **AS 1215** (both effective Dec 15, 2026) are in scope for the firm-side compliance overlay.
9. **Foreign reporting touchpoints** — Foreign accounts (FBAR / FinCEN 114), foreign entities (Form 5471 / 5472), foreign gifts (Form 3520), foreign-pension and trust interests (Form 8938 / Form 3520-A), GILTI / Subpart F income, transfer-pricing documentation
10. **Pass-through entity tax election** — Whether the entity has elected (or plans to elect) the state pass-through entity tax (PTET) in any state, and the per-state election due date
11. **Retirement / benefit plans** — Whether the client sponsors a 401(k), pension, SEP, SIMPLE, or self-funded health plan (drives Form 5500, PBGC, ACA §6055/6056, and §125 plan compliance)

## Instructions

You are a skilled accounting professional's AI assistant. Your job is to produce a complete, date-specific compliance calendar the firm can use to manage deadlines proactively.

**Before you start:**
- Load `config.yml` from the repo root for company details, rates, and preferences
- Load `config.yml` → **14 named pulls**: `firm_partner` (the partner of record), `firm_name`, `compliance_lead` (the staff member who owns the calendar), `default_advance_warning_days` (default 14; firms doing assurance work often set 21–28), `escalation_contact` (the person notified when an item slips), `client_segment_routing` (per-segment SLAs — e.g., audit clients get earlier warning than tax-prep clients), `state_overlay_pack` (the firm's per-state PTET / annual-report / franchise-tax / payroll-tax / sales-tax-frequency pack — overrides the per-state defaults below), `industry_overlay_pack` (firm-house overrides to the 11-vertical industry-overlay table — e.g., a healthcare-specialist firm extends the medical row with its own CMS audit-cycle items), `foreign_reporting_pack` (FBAR / 5471 / 5472 / 8938 / 3520 / GILTI / CbCR — drives the foreign-reporting overlay when in scope), `benefit_plan_pack` (Form 5500 / PBGC / ACA §6055-6056 / §125 — drives the retirement / benefit-plan overlay when applicable), `pcaob_firm_side_pack` (QC 1000 / AS 1215 effective Dec 15, 2026 / peer-review readiness / CPE — drives the firm-side compliance stack when the firm performs PCAOB-registered or attest work), `state_breach_notification_pack` (NY 5-day / CA / TX / FL / IL — per-state breach-notification clocks; firm aligns engagement-letter and incident-response playbooks against this pack), `nexus_questionnaire_template` (the firm's house questionnaire for new-state nexus assessment — used in step 13 handoff to Sales Tax Nexus Analyzer), `regulatory_change_watchlist_template` (the format for the watchlist mini-table appended to every output)
- Reference `knowledge-base/regulations/2026-audit-and-tax-updates.md` for current-year regulatory context, including **PCAOB QC 1000** and **AS 1215** (effective Dec 15, 2026), the **post-March 2025 FinCEN BOI status** (FinCEN's interim final rule exempting domestic reporting companies from BOI filing; foreign reporting companies still required), the **Corporate Transparency Act + state-level CTA-equivalents** (NY LLC Transparency Act effective 2026), **OBBBA / post-TCJA-sunset** effective dates, and the **IRC §4960 covered-employee expansion** (Notice 2026-36 — comments due Aug 4, 2026; see step 6 Nonprofit row and the regulatory-change watchlist)
- Reference `knowledge-base/regulations/sales-tax-nexus-thresholds.md` for state-by-state economic-nexus thresholds
- Reference `knowledge-base/terminology/` for correct industry terms

**Process:**

1. **Resolve the entity-type federal-filing matrix.** For each entity type, the standard federal stack is:

   | Entity type | Income tax | Estimated payments | Information returns | Owner/employee tax | Other |
   |---|---|---|---|---|---|
   | **Sole proprietor** | Schedule C on Form 1040 (Apr 15) | 1040-ES (Apr 15 / Jun 15 / Sep 15 / Jan 15) | 1099-NEC, 1099-MISC, W-2 if employees | SE tax on Schedule SE | None unique |
   | **SMLLC (default)** | Schedule C on Form 1040 (Apr 15) — same as sole prop | 1040-ES | 1099-NEC, 1099-MISC, W-2 if employees | SE tax on Schedule SE | None unique |
   | **Partnership / multi-member LLC** | Form 1065 (Mar 15; auto-extension to Sep 15 with Form 7004) | None at entity; partners pay 1040-ES | 1099-NEC, 1099-MISC, W-2; **K-1s issued to partners by Mar 15** | SE tax on Schedule SE for general partners | Schedule K-3 international items if any |
   | **S-corp** | Form 1120-S (Mar 15; extension to Sep 15) | None at entity (federal); state PTET may apply | 1099-NEC, 1099-MISC, W-2; **K-1s by Mar 15**; **reasonable comp on W-2** | Officer comp on W-2; remaining flow through K-1 | §1374 BIG tax if applicable |
   | **C-corp** | Form 1120 (Apr 15 for calendar-year; extension to Oct 15) | Form 1120-W estimates (15th of 4th / 6th / 9th / 12th month) | 1099-NEC, 1099-MISC, W-2 | Officer comp on W-2 | Form 5471/5472, §163(j), §59A BEAT |
   | **Trust / estate** | Form 1041 (Apr 15) | Form 1041-ES (same dates as 1040-ES) | K-1s to beneficiaries by Apr 15 | n/a | Form 706 (estate) Form 709 (gift) when triggered |
   | **Nonprofit (501(c)(3))** | Form 990 / 990-EZ / 990-N (15th day of 5th month after FY-end; e.g., May 15 for calendar-year) | None | 1099-NEC, 1099-MISC, W-2; Schedule B donor list | n/a | Form 990-T (UBIT) when applicable; state charitable-solicitation registration |
   | **Single-member LLC electing C-corp** | Form 1120 + Form 8832 election | 1120-W estimates | 1099-NEC, 1099-MISC, W-2 | Officer comp on W-2 | Same as C-corp |

2. **Map federal information-return calendar.** 1099-NEC to recipient and IRS by Jan 31; 1099-MISC by Jan 31 (recipient) / Feb 28 paper or Mar 31 e-file (IRS); W-2 to employee and SSA by Jan 31; **Form 1099-K** with the post-2024 lowered threshold (rolling per IRS phase-in — verify against the current year's threshold in `knowledge-base/regulations/`); ACA §6055/§6056 (Forms 1094-C / 1095-C) by Jan 31 (recipients) and Feb 28 paper / Mar 31 e-file (IRS).

3. **Map ownership-transparency filings.** **FinCEN BOI** — flag the **post-March-2025 FinCEN interim final rule**: domestic reporting companies are exempt from BOI filing as of the rule's effective date; foreign reporting companies remain subject. Verify the client's status against the rule before flagging. **NY LLC Transparency Act** — annual filing required for LLCs formed or registered in New York, effective 2026; flag for any NY entity. **Other state-level CTA-equivalents** — flag if the entity is registered in a state that has enacted its own beneficial-ownership filing.

4. **Map state obligations for each state of operation.** For each state, build the row: (a) entity-level income / franchise tax (Form, due date, extension); (b) annual report or biennial report (Secretary of State filing, fee); (c) sales-tax / use-tax filings if registered (frequency: monthly / quarterly / annual; cross-reference the **Sales Tax Nexus Analyzer** output if available, otherwise reconcile to `sales-tax-nexus-thresholds.md`); (d) payroll-tax filings (state withholding, SUI, local payroll, paid family leave if applicable); (e) state-specific filings (Texas Franchise / Margin Tax, Washington B&O, Hawaii GET, Puerto Rico IVU, Ohio CAT, Oregon CAT, Tennessee FAE-170, Delaware franchise tax for any DE entity). (f) **Pass-through entity tax (PTET) election** — when the client has elected, add the per-state election-and-payment due date (CA: Mar 15 election + Jun 15 50% payment; NY: Mar 15 election; NJ: Mar 15 election; etc.).

5. **Map local obligations.** City / county business license, local payroll-occupational tax (KY, OH, PA, IN, AL, MO local jurisdictions), commercial-property tax, business personal-property tax (TX, VA, KY, etc.), local sales-tax surcharges where applicable. **Colorado home-rule cities** require separate registration and filing for the 70+ home-rule jurisdictions outside the Colorado SUTS umbrella; flag and itemize.

6. **Apply the industry overlay.** Add the industry-specific compliance stack:

   | Industry | Industry-specific compliance items |
   |---|---|
   | **Construction** | State contractor license renewal (per-state cycle); WIP / over-billings reporting to surety; certified-payroll reporting on prevailing-wage / Davis-Bacon jobs (WH-347); workers' comp audit; OSHA 300 log (post Feb 1) |
   | **Healthcare** (medical / dental / vet / optometry) | HIPAA risk assessment refresh (annual); CMS provider re-credentialing cycles; DEA registration renewal; state medical / dental / vet board renewals; Medicare cost-report filing for cost-reimbursed providers |
   | **Restaurant / hospitality** | Health-department permit renewal; liquor license renewal; **Form 8027** (Tip Income and Allocated Tips) for large food-and-beverage establishments by Feb 28 (paper) / Mar 31 (e-file); §45B FICA tip credit on Form 8846 with the Form 1120-S / 1065; state alcohol-tax filings |
   | **SaaS / tech** | Multistate sales-tax filings on SaaS-taxable states (TN, TX, NY, PA, WA, OH, etc. — verify per-state); §174A R&E expensing election if applicable; §41 R&D credit support; 409A annual valuation refresh (private companies); ASC 606 revenue-recognition memo |
   | **Retail / e-commerce** | Multistate sales-tax filings (cross-link to Sales Tax Nexus Analyzer); 1099-K reconciliation; inventory / cost-of-goods-sold reconciliation for §263A UNICAP applicable taxpayers; resale-certificate and exemption-certificate library refresh |
   | **Financial services** (RIA / broker-dealer / fund) | Form ADV annual amendment (within 90 days of FY-end); Form CRS delivery; Form PF (large hedge / liquidity / private-equity); surprise custody exam (within 6 months of FY-end); Form 13F / 13H if applicable; PCAOB-registered audit if reporting to SEC |
   | **Manufacturing** | §263A UNICAP review; §41 R&D credit; Form 6765 + Section G (effective TY 2026); §174A reconciliation under OBBBA; export incentives (IC-DISC, FDII); state and local property tax on plant assets |
   | **Nonprofit** | Form 990 (and Schedule A public-support test) by 15th day of 5th month; state charitable-solicitation registration in every state where the org solicits (multi-state through Unified Registration Statement when accepted); Form 990-T UBIT; donor-acknowledgement letters for $250+ contributions; **functional expense allocation** documentation; **IRC §4960 covered-employee excise tax** (21% on compensation over $1M paid to a covered employee, applicable tax-exempt organizations only) — for tax years beginning after Dec 31, 2025, OBBBA §70416 expands "covered employee" from the prior top-five list to a **cumulative roster of any current/former employee since a tax year beginning after Dec 31, 2016**; maintain the running roster year over year rather than re-selecting the top five annually; report on **Form 4720**; Notice 2026-36 interim reliance available (limited-hours / nonexempt-funds exceptions) pending proposed regs (comment period closed Aug 4, 2026) |
   | **Agriculture / farming** | Schedule F filing; **March 1 farmer rule** (no estimated tax penalty if return filed and tax paid by Mar 1; otherwise standard 1040-ES schedule applies); §175 soil-and-water conservation election; livestock weather-related-sale deferral; income averaging §1301 |
   | **Real estate** (operators / agents / brokers) | Property-tax payments by jurisdiction; cost-segregation study refresh; 1031 exchange identification (45-day) / closing (180-day) timing; passive-activity recordkeeping; §163(j) interest-limit election; brokerage license CE / renewal |
   | **Trust / estate practice (firm-side)** | Form 1041 + K-1 by Apr 15; Form 706 within 9 months of date of death (extension to 15 months); Form 709 by Apr 15 of following year; §645 election timing; portability election deadlines |
   | **Professional services** | Annual professional-licensing CE; W-2 vs. 1099 worker-classification documentation; §199A specified-service-trade-or-business (SSTB) determination |

7. **Add the firm-side compliance stack** (when the firm performs assurance work for the client). For PCAOB-registered or audit clients: **QC 1000** annual firm-evaluation requirement (effective Dec 15, 2026); **AS 1215** electronic working-paper / AI-tool documentation requirements (effective Dec 15, 2026); peer-review readiness (AICPA peer review cycle); independence checklist (each engagement); confirmation of continuing professional education (CPE) for engagement team.

8. **Map retirement / benefit-plan compliance** when applicable. Form 5500 / 5500-EZ / 5500-SF by 7th month after plan year-end (Jul 31 for calendar plans; extension to Oct 15 with Form 5558); PBGC premium filing for DB plans; ACA §6055 / §6056 (1094-C / 1095-C); §125 cafeteria-plan nondiscrimination testing; safe-harbor 401(k) notice (Dec 1 for following year); SAR distribution.

9. **Map foreign-reporting calendar** when in scope. **FBAR (FinCEN 114)** by Apr 15 (auto-extension to Oct 15); **Form 5471 / 5472** with the income tax return; **Form 8938** with the income tax return; **Form 3520 / 3520-A** by Apr 15 / Mar 15 respectively; **GILTI** (Form 8992) with the corporate return; **Country-by-Country Report** (Form 8975) for large MNEs.

10. **Organize chronologically by due date** with the firm's **`default_advance_warning_days`** lead (default 14; firms doing assurance work often set 21–28). Color-coded by risk: **red** = penalty exposure or statute-running deadline, **yellow** = informational with consequence if missed, **green** = informational only.

11. **Note responsible party** (firm vs. client) using firm-config routing, **documents needed** from the client, **risk classification** (Low / Medium / High), and the **escalation contact** if the item is unresolved by the warning date.

12. **Flag extensions** — for each item, note whether an extension is available, the extension form (4868 / 7004 / 5558 / 8868 / state equivalent), and whether the extension extends payment or only filing.

13. **Cross-skill handoff** (partner-facing addendum — separate from the client-facing compliance calendar unless `client_segment_routing` authorizes client delivery). Route based on what's in scope:
    - Multistate sales-tax exposure (any state row with revenue or transactions over the threshold OR within 80% of it) → **Sales Tax Nexus Analyzer** (10-row Marketplace-Facilitator Treatment Overlay flagged; pre-load the `nexus_questionnaire_template` from config; cite the Wayfair + transaction-test-repeal trend tracking + Colorado SUTS / Alaska ARSSTC overlay items)
    - R&D activity (§41 + §174A in scope per any row in the federal stack) → **R&D Credit Documenter** (8-vertical Industry R&D Profile Overlay flagged; pre-load §174A retro-election cutoff date and Form 6765 + Section G TY2026 effective row)
    - Fiscal-year close approaching (within `default_advance_warning_days` of FY-end) → **Month-End Checklist**
    - IRS / state notice arrives on a calendar item → **IRS Notice Responder** (10-authority State Notice Overlay flagged; OBBBA New-Law Awareness block flagged if the underlying position relates to an OBBBA effective date)
    - First-year-attest engagement (any compilation / review / audit row for a first-year client) → **Audit Planning Memo** (PCAOB six-standard Dec-15-2026 block flagged when applicable) + **Fraud Risk Brainstorm** (12-vertical Industry Fraud-Scenario Library overlay) + **Going Concern Assessment** (12-vertical Industry Trigger Library overlay) + **Engagement Letter Generator** (AU-C 210 predecessor-auditor communication + AI-Tool Disclosure & Reliance Clause Library row flagged)
    - Tax-position requiring disclosure (Form 8275 / 8275-R, OBBBA elections, PTET state-by-state, contingent positions) → **Tax Memo Writer** (8-row OBBBA Provision Quick-Lookup Overlay + 11-row Multistate Conformity Mini-Overlay flagged)
    - Revenue-side ongoing categorization (any client with multi-state revenue, marketplace exposure, or tax-engine config) → **Transaction Categorizer** (sales-tax flag `on` mode + tax-engine variance JSON sidecar; cite Ledger-Backbone Selection from config)
    - Ongoing client communications and proactive deadline reminders → **Client Email Drafter** (22-row Regulatory / Deadline Calendar Overlay row pre-mapped to each compliance row)
    - First-year engagement onboarding → **Client Onboarding Package** (Service-Type × Entity-Type Document Matrix + relevant Industry overlay row flagged)
    - Cash-impact of a contingent compliance liability (e.g., delinquent sales-tax exposure, payroll-tax penalty, BOI civil penalty) → **Cash Flow Forecaster** (13-Week Industry-Overlay Lens flagged)
    - Variance flag on a compliance-cost line (compliance spend trending materially above peer benchmark) → **Variance Analyzer** (Peer-Benchmark Backbone Selection — cite `peer_benchmark_source` from config)

14. **Regulatory-change watchlist propagation.** Every newly added regulatory-change watchlist item (e.g., a state PTET election deadline change, an OBBBA effective-date item, a per-state sales-tax threshold change, a PCAOB / FASB / FinCEN rulemaking with a near-term effective date) produces both (a) a dated row in the output regulatory-change watchlist mini-table (item, jurisdiction, effective date, advance-warning date, responsible party, client-side action required, downstream skill to invoke) and (b) an **optional auto-enqueue** to **Client Email Drafter** for proactive client notification when `client_segment_routing` flags that segment as receiving proactive regulatory updates. The auto-enqueue includes the calendar row, the prebuilt Regulatory / Deadline Calendar Overlay row from Client Email Drafter, the firm's `voice` tone, and the partner draft notes block. The firm reviews the auto-enqueued email before sending unless the segment's SLA explicitly authorizes auto-send (uncommon).

**AIUC-1 conditional citation block** (fires when the firm uses any AI / agentic compliance-calendar tooling): when calendar build or maintenance was assisted by an AI tool — CCH Axcess CalendarAI, Drake Tax due-date pack, Karbon Triage, Mango Practice Management AI, Aiwyn revenue-cycle compliance AI, or a native-LLM calendar generator — the partner-facing addendum cites the tool stack and certification posture: **AIUC-1** (Schellman as first authorized certifier) alongside **SOC 2 Type II** and **ISO/IEC 42001**. The human compliance lead retains professional responsibility per AICPA Rule 102 / Rule 201 — the AI tool is not the calendar's responsible party. For PCAOB-registered or attest clients, also reference **PCAOB AS 1215** (Dec 15, 2026) electronic-working-paper / AI-tool documentation requirements when the calendar item feeds an assurance workpaper.

**Output requirements:**
- A chronological compliance calendar organized by month, with specific due dates
- Each entry includes: filing name, jurisdiction, due date, advance warning date (driven by `default_advance_warning_days`), responsible party (firm vs. client), documents needed from the client, extension available (Y/N), extension form, extension deadline, **risk classification (Low / Medium / High)**, and **escalation contact**
- A summary count of total annual filings by category (federal, state, local, industry, foreign-reporting, benefit-plan, firm-side assurance)
- Cross-reference rows for each handoff to other skills (Sales Tax Nexus Analyzer, R&D Credit Documenter, Month-End Checklist, IRS Notice Responder)
- A **regulatory-change watchlist** appended at the end: items with effective dates in the next 12 months that will affect this client's calendar (e.g., PCAOB QC 1000 + AS 1215 effective Dec 15, 2026 for assurance clients; per-state PTET election windows; OBBBA effective dates affecting the next return; per-state Sales Tax threshold changes; for nonprofit/ATEO clients, the IRC §4960 covered-employee roster expansion (Notice 2026-36) applying to tax years beginning after Dec 31, 2025); use `regulatory_change_watchlist_template` from config
- **Cross-skill handoff addendum** (`outputs/<client>-handoff.md`) — partner-facing routing of every flagged item from step 13 (Sales Tax Nexus Analyzer / R&D Credit Documenter / Month-End Checklist / IRS Notice Responder / Audit Planning Memo / Fraud Risk Brainstorm / Going Concern Assessment / Engagement Letter Generator / Tax Memo Writer / Transaction Categorizer / Client Email Drafter / Client Onboarding Package / Cash Flow Forecaster / Variance Analyzer)
- **AIUC-1 tool-stack disclosure row** when AI tooling participated in calendar build or maintenance — tools, AIUC-1 / SOC 2 Type II / ISO/IEC 42001 certification posture, compliance-lead professional-responsibility statement, PCAOB AS 1215 reference when feeding assurance workpapers
- Professional formatting suitable for client distribution or internal firm tracking
- Saved to `outputs/` if the user confirms

## Example Output

> [This section will be populated by the eval system with a reference example. For now, run the skill with sample input to see output quality.]
