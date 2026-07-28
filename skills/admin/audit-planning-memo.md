---
name: "Audit Planning Memo"
category: admin
tools: [claude, chatgpt]
difficulty: advanced
time_saved: "~60 min/engagement"
version: 2.3
last_eval_score: 9.0
---

# 🔍 Audit Planning Memo

## Purpose

Draft a risk-based audit planning memorandum that satisfies AU-C 300 (Planning an Audit) and AU-C 315 (Understanding the Entity and Its Environment and Assessing the Risks of Material Misstatement). The memo documents the engagement team's understanding of the client, identifies risks of material misstatement at the financial-statement and assertion level, sets materiality and performance materiality, and lays out the planned audit approach — reliance on internal controls vs. fully substantive, nature/timing/extent of procedures, significant risks, and required team composition. Designed for GAAS engagements at private companies; can be adapted for Yellow Book, PCAOB, or ERISA audits with appropriate cross-references.

## When to Use

Use this skill at the start of every new audit, review, or examination engagement, and whenever the partner updates the audit strategy mid-engagement (e.g., after discovering a control deficiency, after a significant acquisition, after management turnover). Also useful for internal audit functions producing a risk-based annual plan.

## Required Input

Provide the following:

1. **Engagement basics** — Client name, fiscal year-end, prior-year auditor (if any), report type (GAAS audit, Yellow Book, PCAOB, ERISA, AUP), user of the report (bank, shareholders, regulator, grant agency), and filing deadline.
2. **Entity profile** — Industry and sub-industry, legal structure, locations and subsidiaries, size (revenue, total assets, employees), ownership structure, key management personnel, and any relationships with affiliates (related parties).
3. **Operations overview** — Revenue model, top three to five revenue streams, key customers and vendors, production / service delivery cycle, significant estimates (allowance for credit losses, warranty, inventory reserves, goodwill, deferred tax valuation), and any unusual transactions (business combinations, divestitures, debt refinancing, equity issuance, related-party transactions).
4. **Prior-year audit file summary** — Areas with prior-period adjustments, management letter comments, material weaknesses or significant deficiencies, and any disagreements with management.
5. **Internal control environment** — Segregation of duties, IT general controls, key business-process controls for revenue, purchasing, payroll, treasury, and financial close. SOC 1 reports for outsourced services. Whether the team plans to rely on controls or take a fully substantive approach.
6. **Fraud-risk indicators** — Management override potential, revenue-recognition pressures (performance-based comp, debt covenants, IPO path), significant estimates susceptible to bias, related-party transactions, unusual journal-entry patterns.
7. **Engagement economics** — Budgeted hours by staff level, partner and manager assigned, specialist needs (IT audit, tax, valuation, actuarial), timing (interim vs. year-end fieldwork).

## Instructions

You are a skilled accounting professional's AI assistant specializing in risk-based audit planning. Your job is to produce a planning memorandum that a partner can review, edit, and sign. Never assert a conclusion the input doesn't support — when a fact is missing, mark the item "[INFO NEEDED]" rather than guessing.

**Before you start:**
- Load `config.yml` for firm name (`firm_name`), partner/manager names (`firm_partner`), standard materiality policy (`default_materiality_policy`, `performance_materiality_pct`, `clearly_trivial_threshold_pct`), risk-rating scale (`risk_rating_scale`), and engagement-letter references. Pull `pcaob_or_gaas_default`, `it_audit_specialist`, `tax_specialist`, `valuation_specialist`, `industry_overlay_pack`, and `entity_type_overlay_pack` to auto-route downstream steps. Also pull `audit_documentation_ai_stack` (the firm's governed source-linked audit/engagement tooling, e.g. `caseware-verity`, `cch-axcess-audit`, `cch-axcess-engagement`, `suralink`, `datasnipper`, `mindbridge`, `caseware-sherlock`, or `none`) and `aiuc1_disclosure_default`; these drive which tools are named in the AI-tool-reliance documentation (entity-and-environment narrative, ITGC / AS 2110 ¶.05 risk identification) and the AIUC-1 block.
- Reference `knowledge-base/regulations/` for AU-C and PCAOB citations. For any **PCAOB in-scope engagement** (issuer or voluntary adopter), cite the full **Dec-15-2026 six-standard modernization block** as a single integrated package — not any individual standard in isolation:
  - **QC 1000** (firm-level quality management, effective Dec 15, 2026)
  - **AS 1215** (engagement-level documentation, effective Dec 15, 2026)
  - **AS 2110** ¶.05 and ¶.41 amendments (risk identification including ITGC, AI / agentic-tool risks, and personnel-competency gaps; client-acceptance / retention feeds the assessed-risk register; effective Dec 15, 2026)
  - **AS 2201** (ICFR conforming amendments, effective Dec 15, 2026)
  - **AS 1220** (engagement quality reviewer modernization — EQR must now evaluate significant judgments and engagement-team deficiency responses; effective Dec 15, 2026)
  - **AS 2901** — **retitled** (not renumbered) from *"Consideration of Omitted Procedures After the Report Date"* to *"Responding to Engagement Deficiencies After Issuance of the Auditor's Report"*, and substantively expanded: it now addresses both (a) deficiencies where sufficient appropriate audit evidence was **not** obtained (auditor must perform additional procedures) and (b) other engagement deficiencies identified after issuance (auditor must act in light of the deficiency's nature and severity), integrated with QC 1000 monitoring-and-remediation; effective Dec 15, 2026. It **relates to** AS 2905 ¶.06–.09 (*Subsequent Discovery of Facts Existing at the Date of the Auditor's Report*) but is **not** a redesignation of AS 2905, and AS 2905 remains a separate standard — do not describe AS 2901 as "renamed from" or "redesignated from" any other AS number.
  - **AIUC-1 conditional citation**: if the firm or the client uses AI / agentic tools in the financial-reporting or audit process, note that AIUC-1 (AI governance certification standard; Schellman as first authorized certifier; quarterly-update cadence) is one signal — alongside SOC 2 Type II and ISO/IEC 42001 — when documenting AI-tool governance under AS 2110 ¶.05 and QC 1000. Do not cite AIUC-1 alone; always pair with the firm's applicable conformance regime. When the firm relies on a **governed, source-linked audit/engagement platform** for any planning or execution step, name the specific tool from `audit_documentation_ai_stack` and the audit-trail / human-oversight feature it provides — the source-linked governed-tooling options include **Caseware Verity** (Governed Decision Environment — citation-backed, reviewable, traceable before output enters the engagement file), **CCH Axcess Audit** (agentic workflows combining firm methodology + firm data + document context under human oversight; AICPA ENGAGE 2026 showcase, June 11, 2026), **CCH Axcess Engagement Document Analysis Agents** (board-minute / contract review → structured summaries, risk flags, chat follow-ups), **Suralink Workpaper Suite Intelligence** (Extract Links / Link Answers source-linked extraction), **DataSnipper**, **MindBridge**, and **Caseware Sherlock**. The skill file itself remains the firm's methodology specification and the citation/review layer over what any such agent produces.
- Reference `knowledge-base/best-practices/` for the firm's risk-assessment template.
- If a **Fraud Risk Brainstorm** has already been produced for this engagement, load it so its identified fraud risks feed directly into the significant-risk table here. If a **Going Concern Assessment** exists or is anticipated, cross-reference it from the entity-and-environment narrative.

**Industry Risk Overlay** (resolve to the client's vertical before step 5):

| Vertical | Primary RMM focus | Presumed fraud-risk posture | Key specialist likely needed |
|---|---|---|---|
| **SaaS / subscription** | Revenue recognition (ASC 606 variable consideration, cut-off on multi-year ARR contracts, deferred-revenue roll); capitalized internal-use software (ASU 2025-06 agile-iteration boundary) | Channel-stuffing / side-letter presumption; ARR inflation pressure from investor metrics | Tax (§174A R&D); IT audit (dev-pipeline controls) |
| **Professional services** | WIP / unbilled receivables valuation; partner-note / related-party transactions; revenue cut-off on fixed-fee engagements | Fictitious time entries to hit utilization targets; early revenue recognition on long-term contracts | None typically; IT audit if large ERP |
| **Retail / e-commerce** | Inventory existence and valuation (FIFO vs. weighted-average); sales returns reserve; marketplace-platform revenue netting; gift-card liability rollforward | Inventory overstatement to support covenant compliance; channel-mix manipulation | IT audit (marketplace integrations, chargebacks) |
| **Construction** | POC revenue (over/under billings); retainage; job-cost allocation; bonding-covenant compliance; related-party subcontract terms | Front-loaded POC to maximize progress billings; fictitious change orders; related-party subcontractor markups | Tax (§460 look-back); IT audit (job-cost system) |
| **Restaurant / hospitality** | Cash and tip-revenue existence; prime-cost % (food + labor); FICA tip credit (§45B); multi-location intercompany eliminations | Skimming of cash receipts; tip-reporting manipulation; fictitious vendor invoices for food costs | Tax (Form 8027 / §45B); payroll |
| **Manufacturing** | Inventory costing (standard cost, UNICAP §263A, overhead absorption); warranty reserve; capitalized vs. expensed R&D (§174A) | Inventory overstatement via inflated absorption; channel-stuffing at year-end; improper cost capitalization | Tax (§174A / §41 R&D credit); IT audit (ERP bill of materials) |
| **Healthcare** (practice / hospital) | Patient AR net-of-contractual-adjustments and denial reserve; physician compensation alignment with FMV (Stark Law); cost report settlements | Upcoding / false claims risk; related-party physician arrangement abuse | Regulatory (Stark / AKS counsel); actuarial (settlement reserves) |
| **Nonprofit / 501(c)** | Grant-expenditure compliance; restricted vs. unrestricted net-asset classification; functional-expense allocation; Form 990 Schedule A public-support test | Misapplication of restricted grants; UBIT omission; fraudulent disbursements through weak SoD | Tax (990 / UBIT); grant-compliance specialist |
| **Real estate** | Fair value of investment property; lease classification (ASC 842); tenant-concentration risk; related-party debt terms; 1031-exchange deferral accounting | Overstated rent rolls; inflated cap-rate assumptions in goodwill or impairment tests; related-party loans at non-arm's-length rates | Valuation (appraiser); tax (cost-seg / §1031) |
| **Financial services** | Investment valuation (Level 2 / 3); custody-asset existence; regulatory capital and net-capital computation; CECL reserve reasonableness | Mismarked Level 3 positions; management-fee revenue manipulation; undisclosed related-party transactions | Valuation; IT audit; legal (regulatory capital) |
| **Agriculture / farming** | Crop / livestock inventory valuation; commodity price hedging mark-to-market; CCC loan accounting; §175 / §180 expensing elections | Inflated commodity inventory to support operating-line borrowing base; weather-loss overclaims | Tax (§1301 income averaging; §175); commodity broker |
| **Generic fallback** | Revenue recognition, accounts receivable, inventory, and related-party transactions | Revenue fraud presumed per AU-C 240.27; management override mandatory per AU-C 240.32 | Determined by engagement-specific risk |

**Entity-Type Risk Overlay** (resolve to the client's legal structure before step 5):

| Entity type | Equity / capital assertion focus | Related-party scope | Applicable compliance add-ons |
|---|---|---|---|
| **Sole proprietor** | Schedule C tie-out; no equity section; owner draws vs. business expenses | Owner as sole related party; personal vs. business expense commingling | SE tax; §179 / bonus depreciation |
| **SMLLC / disregarded** | Same as sole proprietor; confirm single-member classification not inadvertently changed | Owner and any multi-member reclassification risk | State LLC tax; §199A eligibility |
| **Multi-member LLC / partnership** | §704(b) capital account rollforward by partner; guaranteed payments vs. distributions; §752 debt allocation | All partners and management-fee entities | K-1 accuracy; §704(c) built-in gain; BBA audit-rule applicability |
| **S-corp** | AAA / OAA / E&P reconciliation; single-class-of-stock test for any disproportionate distributions; reasonable-comp wage review | Shareholder-employees; related-party loans (§7872 imputed interest) | §1366 / §1367 basis tracking; PTET election compliance |
| **C-corp** | Retained earnings rollforward; DTA / DTL provision (ASC 740); share-based comp expense | Related parties per ASC 850; executive-comp proxies if SEC filer | §163(j) ATI; BEAT / Pillar Two if multinational |
| **Nonprofit / 501(c)** | Net-asset rollforward (unrestricted / temporarily restricted / permanently restricted); functional-expense allocation | Disqualified persons under §4958; management-fee intermediaries | Form 990; UBIT; grant-compliance |
| **Trust / estate** | Principal vs. income segregation (UPIA); DNI build; fiduciary accounting income | Beneficiaries; trustee self-dealing | §643 distributable net income; fiduciary income tax |
| **Multi-entity group** | Intercompany elimination completeness; consolidation entries; transfer-pricing arm's-length substantiation | All related entities; shared-services arrangements | GAAP consolidation (ASC 810); ASC 740 FIN 48; TP documentation |

**Process:**

1. **Document independence and ethics.** Confirm the engagement team is independent under AICPA Code of Professional Conduct (or PCAOB Rule 3520 for issuers). List any non-audit services and how the firm's independence policy addressed them.
2. **Document the firm's acceptance / continuance decision.** Cite the client-acceptance procedures performed (background checks, prior-auditor communication under AU-C 210, preliminary risk assessment). For continuance clients, note any change in circumstances.
3. **Describe the entity and its environment (AU-C 315).** Produce a concise narrative covering: industry and regulatory factors, nature of the entity (ownership, governance, structure, investments, financing), selection and application of accounting policies, objectives and strategies and related business risks, and measurement of financial performance. Identify the applicable financial reporting framework (US GAAP, IFRS, tax basis, FRF for SMEs).
4. **Set materiality.** Compute overall materiality using the firm's benchmark policy (commonly 5% of pre-tax income for profit-oriented entities, 0.5–1% of total revenue or total assets for break-even or asset-heavy entities, 1–2% of net assets for not-for-profits). Compute performance materiality (typically 50–75% of overall) and clearly-trivial threshold (typically 3–5% of performance materiality). Document the rationale.
5. **Identify significant risks and risks of material misstatement.** For each significant account or disclosure, state the assertions most at risk (existence/occurrence, completeness, valuation/allocation, rights/obligations, presentation). Flag each as significant risk if it involves fraud, complex or subjective judgment, unusual transactions, or related parties. Revenue is a presumed fraud risk under AU-C 240 unless rebutted.
6. **Plan the response.** For each significant risk, specify nature/timing/extent of audit procedures: tests of controls vs. substantive procedures, interim vs. year-end, analytical procedures vs. detail testing, sampling approach, and third-party confirmation strategy (positive vs. negative, AR, cash, debt, investments). For fraud risk of management override, mandate the AU-C 240 journal-entry testing, accounting-estimate bias review, and significant-unusual-transaction review.
7. **Document the engagement team and specialist needs.** Staff assignments, supervision and review plan, required specialists (IT, tax, valuation, actuarial), planned timing and location of fieldwork, and expected date of report issuance.
8. **Set communication plans.** With those charged with governance (AU-C 260) — planned matters to communicate at the start and end of the engagement. With management — interim and year-end status meetings.
9. **Document fraud discussion.** Summarize the engagement team's AU-C 240 brainstorming: where and how the financial statements might be susceptible to material misstatement due to fraud, who in management is in a position to override controls, and the resulting audit responses.
10. **Cross-skill handoffs.** After the sign-off block, list every downstream or companion skill that should be invoked for this engagement — with the specific trigger that fires it:
    - **Fraud Risk Brainstorm** — always produce before finalizing the step-5 risk table; load its output directly into significant-risk rows.
    - **Audit Coverage & Traceability Matrix** — always run before fieldwork closes and again at partner sign-off; it tests whether the audit program planned here actually spans every material financial-statement element and assertion, and returns the coverage-gap list. Hand off this memo's materiality figures and its step-5 significant-account / significant-risk table. If the matrix surfaces a material account or significant risk that is absent from step 5, that is a risk-assessment gap — re-open this memo and re-run the assessed-risk register.
    - **Going Concern Assessment** — trigger if any of the following appear in step 3 or step 5: recurring losses, working-capital deficiency, covenant breach or projected breach, negative operating cash flow, going-concern disclosure in prior-year report, or any downside cash-flow scenario projecting negative cash within 12 months of the expected report-issuance date.
    - **Financial Narrative Builder** — trigger if the engagement team identifies revenue or margin variance > materiality between periods that management will need to explain to lenders, investors, or a board audit committee; hand off the TB and the step-5 significant-account list.
    - **Month-End Checklist** — trigger if planning fieldwork reveals month-end close weaknesses (late JEs, reconciliation gaps, delayed sub-ledger tie-outs); route to close-cadence improvement rather than just documenting the deficiency.
    - **IRS Notice Responder** — trigger if a tax notice, IRS exam, or state examination is open for the same entity during the audit period; coordinate response posture with the audit-planning risk assessment.
    - **R&D Credit Documenter** — trigger if the industry overlay flags R&D activity (SaaS, manufacturing, biotech, medtech) and the client has not yet engaged R&D credit documentation; coordinate §174A treatment with the income-tax provision review.
    - **Sales Tax Nexus Analyzer** — trigger if the industry overlay or the entity-and-environment narrative identifies multi-state activity, marketplace/e-commerce revenue, or a recent nexus-threshold crossing.

**Output requirements:**
- Memo structure: (1) Engagement Information, (2) Independence and Acceptance, (3) Understanding of Entity (including industry overlay row and entity-type overlay row that fired), (4) Materiality, (5) Risk Assessment Summary (table by account/assertion/risk level, populated using the vertical RMM focus from the industry overlay), (6) Significant Risks and Planned Responses, (7) Engagement Team & Specialists, (8) Timing & Budget, (9) Communications, (10) Fraud Discussion, (11) Sign-Off (partner, manager, senior), (12) Cross-Skill Handoff Block (list every triggered companion skill with its specific trigger).
- All cited standards must be real (AU-C section numbers, PCAOB AS numbers if issuer). For PCAOB engagements, cite the full six-standard Dec-15-2026 block (QC 1000, AS 1215, AS 2110, AS 2201, AS 1220, AS 2901) as a single integrated wave — never any individual standard in isolation. If a cite is uncertain, mark "[VERIFY]".
- Materiality computation shown with benchmark, percentage, dollar result, and rationale — not just a final number.
- Risk ratings use the firm's standard scale from `config.yml` → `risk_rating_scale` (typically Low / Moderate / High, with Significant Risk as a separate designation).
- Tone is factual and technical — this is a working paper, not a client deliverable.
- Flag any entity with a going-concern indicator (AU-C 570), ICFR material weakness (AU-C 265), or related-party transaction requiring extended procedures (AU-C 550).
- The industry overlay row and entity-type overlay row that fired must both appear in section 3; they auto-populate the specialist row in section 7 and the primary-RMM rows in section 5.
- Save to `outputs/audit-planning/{YYYY}-{client-slug}-planning-memo.md`.

## Example Output

> [This section will be populated by the eval system with a reference example. For now, run the skill with a sample client profile to see output quality.]
