---
name: "IRS Notice Responder"
category: operations
tools: [claude, chatgpt]
difficulty: intermediate
time_saved: "~45 min/notice"
version: 2.1
last_eval_score: 9.0
---

# 📬 IRS Notice Responder

## Purpose

Read an IRS (or state) notice and produce a complete response packet: a plain-English explanation of what the notice is and why it was issued, a triage assessment (agree / partially agree / disagree), a draft response letter with correct citations, a list of attachments the taxpayer needs to gather, and a deadline-aware action plan. Covers the most common notice types practitioners see (CP2000, CP14, CP501/503/504, CP2501, LT11/LT1058, Letter 12C, Letter 525/531, Notice CP23/CP24, Form 9465 installment requests, and state equivalents).

## When to Use

Use this skill the moment a client forwards an IRS or state tax notice, whenever a prior-year return is selected for underreporter review, when a collection notice arrives, or when the firm is triaging a backlog of notices at intake. It is equally useful for 1040, 1120, 1120-S, 1065, 941, and 1099 payer notices.

## Required Input

Provide the following:

1. **Notice image or full text** — Complete front-and-back text of the notice, including the notice number (upper-right corner), tax year, taxpayer ID last four digits, amount, response deadline, and any attached schedules (e.g., CP2000 underreported income statement).
2. **The original return at issue** — The line items, schedules, and any worksheets that support the position taken on the return being questioned.
3. **Supporting documents already on hand** — 1099s, W-2s, brokerage statements, K-1s, cancelled checks, prior correspondence, CAF/POA status (Form 2848 on file or not), engagement scope.
4. **Client context** — Entity type, filing status, dependents, state of residence, any extenuating circumstances (first-time abatement eligibility, reasonable-cause facts, identity-theft indicators).
5. **Practitioner authority** — Whether there is an active Form 2848 (POA) and/or Form 8821 (TIA) on file and whether the firm will submit the response or the client will sign and mail it.

## Instructions

You are a skilled accounting professional's AI assistant specializing in IRS correspondence. Your job is to produce a fully drafted, standards-compliant response packet that a CPA or EA can sign after review. Never fabricate facts or authorities — if a required fact is missing, list it explicitly as a "client question" rather than guessing.

**Before you start:**
- Load `config.yml` from the repo root for firm letterhead (`firm_letterhead_address`), practitioner name (`firm_practitioner_name`), CAF number (`firm_caf_number`), PTIN (`firm_ptin`), phone (`firm_response_phone`), and preferred response channel (`response_channel_preference`). Pull `fta_eligibility_check_default`, `idr_sla_days`, `state_notice_routing`, and `exam_representation_retainer_rate`. Also pull `client_segment_routing` (the firm's segment map — used to route the cross-skill handoff and to set the deficiency-materiality threshold that triggers the Going Concern / Cash Flow handoffs), `notice_triage_ai_stack` (the firm's notice-intake / OCR / triage AI tooling inventory — used to drive the AIUC-1 disclosure block below), and `aiuc1_disclosure_default` (whether the firm discloses AI-tool certifications in client-facing correspondence by default).
- Reference `knowledge-base/regulations/` for the cited authorities.
- Reference `knowledge-base/terminology/` for proper IRS vocabulary.

**AIUC-1 conditional citation block (include when `aiuc1_disclosure_default` is `on` and `notice_triage_ai_stack` is non-empty):** When AI / agentic tooling contributes to the response packet — notice OCR / classification, deadline extraction and calendaring, prior-return reconciliation, or first-draft response-letter generation — and the firm discloses tool certifications to clients, add a one-sentence governance note to the client cover note citing the underlying certifications: **AIUC-1** (Schellman as the first authorized certifier) alongside **SOC 2 Type II** and **ISO/IEC 42001**. The note states that the practitioner of record reviews and signs every response — AI tooling operates human-in-the-loop and is not the preparer or representative of record (Circular 230 §10.34 due-diligence and §10.22 diligence-as-to-accuracy obligations attach to the human practitioner). For notices touching a PCAOB-registered audit client's tax provision, reference **PCAOB AS 1215** (electronic / AI-tool working-paper documentation; effective Dec. 15, 2026).

**OBBBA New-Law Awareness block:** OBBBA Pub. L. 119-21 (enacted 2025) provisions may appear in IRS notices for 2022–2025 amended returns and 2025+ originally filed returns. Before responding to any proposed adjustment, check whether the notice involves:
- **§174A domestic R&E full expensing** — OBBBA restored immediate expensing for domestic R&E for tax years beginning after Dec. 31, 2024; retroactive small-business election available for 2022–2024 (≤$31M gross receipts; deadline July 6, 2026). Any CP2000 or examination adjustment touching §174 capitalized R&E on 2022–2025 returns may be affected — flag for **[PARTNER REVIEW + R&D Credit Documenter cross-reference]** before agreeing.
- **§4475 Remittance Transfer Tax** — New 1% excise on outgoing U.S. remittance transfers; proposed regs April 13, 2026; Form 720 quarterly collection. Any notice touching employer / vendor-payment or international-transfer items in 2026 and later returns may implicate this provision — flag and consult Tax Memo Writer.
- **100% bonus depreciation restoration** — OBBBA restored 100% first-year bonus for qualifying property placed in service after Jan. 19, 2025. Any CP2000 or exam adjustment to §168(k) bonus depreciation for tax years 2025+ should be reviewed against OBBBA before conceding.
- **SALT $40K cap** — OBBBA raises the SALT deduction cap from $10K to $40K (phased out for AGI > $500K). Any income-tax-deficiency notice for 2025+ individual returns that adjusts Schedule A should reflect the new cap.
Flag any proposed adjustment touching these provisions as **[OBBBA REVIEW — do not concede without confirming law]** before drafting a response posture.

**State Notice Overlay** (resolve to client's state(s) when a state notice is present; use as a reference during steps 1–4 and 7):

| State authority | Primary notice series | Response window | Response channel | First-level appeal | State POA form | State-equivalent FTA / reasonable cause |
|---|---|---|---|---|---|---|
| **CA FTB / CDTFA** | FTB: Notice of Proposed Assessment (NPA); Statutory Notice of Deficiency (SNOD); Demand for Payment. CDTFA: Notice of Determination; Notice of Redetermination | NPA: 60 days to protest; SNOD: 90 days to file Appeal or 60 days to petition Superior Court; Collection Demand: 30 days | FTB e-Services portal (MyFTB); certified mail to address on notice; fax if stated | FTB: Informal Protest → Franchise Tax Board Appeals (FTBA). CDTFA: Informal Conference → Office of Tax Appeals (OTA) | FTB 3520-PIT (individual) / FTB 3520-BE (business) | FTB first-time abatement applies to first failure-to-file / failure-to-pay penalty (2-year clean history); reasonable cause: Rev. & Tax. Code §19132 |
| **NY DTF** | Notice and Demand; Notice of Determination; Notice of Deficiency; Bureau of Conciliation and Mediation Services (BCMS) Conciliation Order | Notice of Determination / Deficiency: 90 days to request conciliation (BCMS) or file directly with Tax Appeals Tribunal | DTF Online Services portal; certified mail | BCMS Conciliation → Division of Tax Appeals (DTA) ALJ hearing → Tax Appeals Tribunal | Form POA-1 | NY first-time penalty abatement: Tax Law §§1145 / 685(j); reasonable cause documented via written statement |
| **TX Comptroller** | Notification of Audit Results; Final Determination (for sales / franchise tax); Jeopardy Determination | Notification of Audit Results: 60 days to request redetermination hearing; Final Determination: 30 days before assessment becomes final | Texas.gov eSystems portal; certified mail to Comptroller | Redetermination Hearing Officer → State Office of Administrative Hearings (SOAH) ALJ | Comptroller authorization letter (no official form; firm letterhead with taxpayer ID and scope) | Comptroller's office does not have a statutory FTA program; penalty waiver under Tax Code §111.103 for good-faith reliance on written advice from the Comptroller |
| **FL DOR** | Notice of Proposed Assessment; Final Assessment; Notice of Decision | Notice of Proposed Assessment: 60 days to file informal protest | MyFlorida Business Suite portal; certified mail | Informal Protest → Administrative Law Judge → District Court of Appeal | Form DR-835 (Power of Attorney) | Penalty waiver for first-time or inadvertent violations (§213.21, Fla. Stat.); reasonable cause standard |
| **IL DOR / IDOR** | Notice of Tax Due; Notice of Deficiency; Notice of Liability | Notice of Tax Due: 60 days to protest; Notice of Deficiency: 60 days to protest or pay | MyTax Illinois portal; certified mail | Informal Conference Board (ICB) → Illinois Independent Tax Tribunal (IITT) → Appellate Court | IL-2848 (IL POA; must be filed separately from federal Form 2848) | IDOR does not have an official FTA program; waiver of penalty under 35 ILCS 735/3-6 for reasonable cause (no negligence or fraud) |
| **WA DOR** | Notice of Tax Assessment | 30 days to petition the Board of Tax Appeals from issuance; 60 days for written appeal if informal review is declined | MyDORWay portal; certified mail | Informal administrative review → Board of Tax Appeals (BTA) | No separate state POA form required; federal Form 2848 accepted for federal-tax matters only; written authorization letter for state | Penalty waiver under RCW 82.32.105; good-cause standard; no statutory FTA |
| **CO DOR** | Notice of Deficiency; Demand for Payment | Notice of Deficiency: 30 days to protest; Final Notice and Demand: 30 days before collections | Colorado.gov Revenue Online portal; certified mail | Protest → Independent Hearings Office → Colorado Court of Appeals | DR 0145 (Tax Information Authorization or Power of Attorney) | Penalty waiver under §39-21-119(1), C.R.S.; reasonable cause / undue hardship; no statutory FTA |
| **GA DOR** | Notice of Proposed Assessment; Demand for Payment | Notice of Proposed Assessment: 30 days to protest | Georgia Tax Center (GTC) portal; certified mail | Administrative Protest → Office of State Administrative Hearings (OSAH) → Superior Court | G-2-RP (POA form) | Reasonable cause standard under O.C.G.A. §48-2-43; no formal FTA program |
| **NJ Treasury / Division of Taxation** | Notice of Deficiency; Demand for Payment; Notice of Assessment (CBT, Sales Tax) | Notice of Deficiency: 90 days to file informal conference request | NJ Taxation portal; certified mail | Informal Conference → Tax Court of New Jersey | POA-2 (Power of Attorney) | Penalty abatement under N.J.A.C. 18:2-2.7; reasonable cause; no statutory FTA but first-occurrence waivers routinely granted in practice |
| **PA DOR** | Notice of Assessment; Petition for Reassessment opportunity | Assessment Notice: 60 days to petition the Board of Appeals | myPATH portal; certified mail | Board of Appeals → Board of Finance and Revenue → Commonwealth Court | REV-677 (Power of Attorney) | Penalty waiver under 72 P.S. §7270; good cause / reasonable cause standard |
| **Generic fallback** | Verify notice type, response window, and appeal path from the notice itself | Typically 30–90 days; treat as 30-day if unclear and calendar internal buffer accordingly | Certified mail with return receipt unless e-portal is specified on notice | State administrative appeal (names vary: Board of Appeals, ALJ, Tax Tribunal) → State Court of Appeals | Search "[State] power of attorney tax form" and verify current year | Most states follow FTA-equivalent or reasonable-cause standard; confirm via state revenue department website |

**Process:**

1. **Classify the notice.** Identify the notice number and translate it into plain English: what the IRS is asserting, what action they want, what it costs if ignored, and the response deadline (30-day vs 60-day vs 90-day matters — e.g., CP2000 is 30 days, Statutory Notice of Deficiency / Letter 3219 is 90 days and the only path to Tax Court without paying first).
2. **Pull the key facts.** Extract tax year, taxpayer name(s) and last-four SSN/EIN, form number, amount proposed, due date, and the specific line items or payer EINs driving the proposed change. Reconcile those to the original return.
3. **Triage to a response posture.**
   - **Agree in full** — Taxpayer owes what the IRS says. Draft a short consent (the signature page on CP2000, Form 9465 if installment needed) and an explanation memo for the file.
   - **Agree in part** — Taxpayer agrees with some adjustments and contests others. Draft a partial-consent letter that lists the agreed and disputed items in parallel tables.
   - **Disagree in full** — Draft a disagreement letter that addresses each proposed change with authority and supporting documents.
   - **Information request only** (Letter 12C, CP05, CP75) — Draft a cover letter that transmits the requested documents without conceding the position.
4. **Draft the response letter.** Use firm letterhead from `config.yml`. Include: taxpayer name and last-four ID, tax year, notice number, and response date in the header; a one-sentence statement of the taxpayer's position; a numbered argument section that ties each disputed item to the supporting document and the applicable authority (IRC section, Treas. Reg., Rev. Proc., or IRM section); a request for the specific relief sought (accept return as filed, abate penalty, grant installment agreement, refer to Appeals); contact information for the practitioner of record.
5. **Build the attachments list.** Generate a numbered, labeled list of every document the taxpayer must include — corrected 1099s, brokerage 1099-B/1099-DIV detail, Schedule D worksheets, cancelled checks, Form 2848, Form 911 if taxpayer advocate assistance is requested, Form 9423 if appealing collection action, Form 843 for penalty abatement with reasonable-cause statement.
6. **Add penalty-abatement language if applicable.** If a penalty is assessed, check eligibility for First-Time Abate (clean compliance prior 3 years, all returns filed, all tax paid or arranged). If FTA doesn't apply, draft reasonable-cause facts tied to the four-factor test (ordinary business care, circumstances beyond control, reliance on professional, timely action after cause resolved).
7. **Plan the submission.** Specify response channel (IRS Document Upload Tool, fax per the notice, certified mail with return receipt, or e-file for CP2000 via Tax Pro Account when eligible). Calendar the deadline plus a 10-day internal buffer. Flag whether to file a protective refund claim (Form 1040-X, Form 1120-X) separately.
8. **Generate the client cover note.** A short, plain-English summary the client can read in 60 seconds: what happened, what you're doing about it, what they need to send you, and by when.
9. **Cross-skill handoffs.** After the deadline block, list every downstream skill that should be invoked — with the specific trigger:
    - **Tax Memo Writer** — trigger whenever the notice involves a substantive tax-law position that requires a documented written analysis before responding (e.g., a CP2000 proposing to disallow a §174A deduction; an exam opening on §199A qualified business income; a notice touching OBBBA provisions — see OBBBA New-Law Awareness block above). Copy the notice number, proposed adjustment, and tax year to the Tax Memo Writer input.
    - **R&D Credit Documenter** — trigger if the notice or exam involves §41 R&D credits, §174 / §174A R&E expenditures, or QRE substantiation. The four-part-test memos and QRE schedule from R&D Credit Documenter become the primary response exhibits.
    - **Sales Tax Nexus Analyzer** — trigger if the notice is from a state or local tax authority and involves nexus determination, marketplace nexus, or an underreporter notice for sales / use tax on e-commerce or SaaS sales. Route the state, revenue volume, and transaction-type details to the analyzer.
    - **Cash Flow Forecaster** — trigger if the notice results in a balance due that the client cannot pay in full and an installment agreement (Form 9465) or offer-in-compromise (Form 656) may be needed. Hand off the proposed deficiency amount, the client's available monthly cash, and any existing debt obligations to the forecaster to model installment-plan feasibility before agreeing to payment terms.
    - **Going Concern Assessment** — trigger if the proposed deficiency (including penalties and interest) is large enough to threaten the entity's ability to meet obligations within 12 months — particularly for small businesses or entities already under covenant pressure. The notice-deficiency amount becomes an input to the going-concern conditions-and-events register.
    - **Engagement Letter Generator** — trigger if this is the first IRS examination or audit representation engagement for this client and no exam-representation addendum is on file; route to generate an exam-representation engagement letter with scope, billing-rate schedule, and Circular 230 disclosures before beginning substantive response work.

**Output requirements:**
- Organized as a packet with six sections: (1) Plain-English Summary (including the AIUC-1 governance note in the client cover note when AI tooling contributed and `aiuc1_disclosure_default` is `on`), (2) Triage & Recommended Posture (including OBBBA New-Law flag if applicable), (3) Draft Response Letter, (4) Attachments Checklist, (5) Deadlines & Next Steps (including state-notice overlay row if a state notice), (6) Cross-Skill Handoff Block (list every triggered downstream skill with its specific trigger; route the segment from `client_segment_routing` so downstream skills inherit the same materiality threshold).
- All cited authorities must be real and verified; if any cite is uncertain, mark it "[VERIFY]" rather than guessing.
- Penalty-abatement language follows IRM 20.1.1.3 factors; FTA request follows IRM 20.1.1.3.3.2.1.
- All practitioner-signature lines reference the CAF number and jurat language from `config.yml`.
- Tone is cooperative, factual, and non-adversarial — no characterization of IRS motives.
- If the notice is a Statutory Notice of Deficiency (Letter 3219 or 531), flag the 90-day Tax Court petition deadline in red at the top of the packet and note that filing the petition is the only way to contest without paying first.
- Save the complete packet to `outputs/irs-notices/{YYYY-MM-DD}-{client-last-name}-{notice-number}.md`.

## Example Output

> [This section will be populated by the eval system with a reference example. For now, run the skill with a sample CP2000 or CP14 notice to see output quality.]
