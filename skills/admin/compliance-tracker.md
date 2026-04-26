---
name: "Compliance Tracker"
category: admin
tools: [claude, chatgpt]
difficulty: intermediate
time_saved: "~20 min/review"
version: 2.0
last_eval_score: 8.8
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
- Load `config.yml` → `firm_partner` (the partner of record), `compliance_lead` (the staff member who owns the calendar), `default_advance_warning_days` (default 14; firms doing assurance work often set 21–28), `escalation_contact` (the person notified when an item slips), and `client_segment_routing` (per-segment SLAs — e.g., audit clients get earlier warning than tax-prep clients)
- Reference `knowledge-base/regulations/2026-audit-and-tax-updates.md` for current-year regulatory context, including **PCAOB QC 1000** and **AS 1215** (effective Dec 15, 2026), the **post-March 2025 FinCEN BOI status** (FinCEN's interim final rule exempting domestic reporting companies from BOI filing; foreign reporting companies still required), the **Corporate Transparency Act + state-level CTA-equivalents** (NY LLC Transparency Act effective 2026), and **OBBBA / post-TCJA-sunset** effective dates
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
   | **Nonprofit** | Form 990 (and Schedule A public-support test) by 15th day of 5th month; state charitable-solicitation registration in every state where the org solicits (multi-state through Unified Registration Statement when accepted); Form 990-T UBIT; donor-acknowledgement letters for $250+ contributions; **functional expense allocation** documentation |
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

13. **Cross-link other skills.** When the client has multistate sales-tax exposure, hand off to **Sales Tax Nexus Analyzer**. When R&D activity is in scope, hand off to **R&D Credit Documenter**. When fiscal-year close is approaching, hand off to **Month-End Checklist**. When an IRS notice arrives, hand off to **IRS Notice Responder**.

**Output requirements:**
- A chronological compliance calendar organized by month, with specific due dates
- Each entry includes: filing name, jurisdiction, due date, advance warning date (driven by `default_advance_warning_days`), responsible party (firm vs. client), documents needed from the client, extension available (Y/N), extension form, extension deadline, **risk classification (Low / Medium / High)**, and **escalation contact**
- A summary count of total annual filings by category (federal, state, local, industry, foreign-reporting, benefit-plan, firm-side assurance)
- Cross-reference rows for each handoff to other skills (Sales Tax Nexus Analyzer, R&D Credit Documenter, Month-End Checklist, IRS Notice Responder)
- A **regulatory-change watchlist** appended at the end: items with effective dates in the next 12 months that will affect this client's calendar (e.g., PCAOB QC 1000 + AS 1215 effective Dec 15, 2026 for assurance clients; per-state PTET election windows; OBBBA effective dates affecting the next return; per-state Sales Tax threshold changes)
- Professional formatting suitable for client distribution or internal firm tracking
- Saved to `outputs/` if the user confirms

## Example Output

> [This section will be populated by the eval system with a reference example. For now, run the skill with sample input to see output quality.]
