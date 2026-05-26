---
name: "Tax Memo Writer"
category: operations
tools: [claude, chatgpt]
difficulty: advanced
time_saved: "~45 min/memo"
version: 2.2
last_eval_score: 8.9
---

# 📝 Tax Memo Writer

## Purpose

Draft a structured tax research memorandum that analyzes a specific tax question, cites applicable IRC sections, Treasury Regulations, and other primary authorities, and arrives at a defensible conclusion with an explicit confidence level, a Form 8275 / 8275-R disclosure posture, and a §6694 preparer-exposure read. Designed to be a file-defensible, partner-signable work product — not a client memo dressed up in tax language.

## When to Use

Use this skill when a client or partner poses a tax question that requires documented research — whether for file documentation, client deliverable, or internal decision support. Common triggers: entity-structure decisions, transaction characterization questions, deduction eligibility, §199A / §179 / bonus-depreciation modeling, state nexus analysis, asset dispositions, compensation structuring, elections (QSBS, §83(b), §754, S-election, check-the-box), accounting-method changes, and any position where the preparer needs written support for §6662 / §6694 purposes. **Especially important for tax years 2026+** because the TCJA individual provisions sunset 12/31/2025 and most have been extended or modified under the One Big Beautiful Bill Act of 2025 (OBBBA) — memos drafted from pre-2026 templates will miss regime changes.

## Required Input

Provide the following:

1. **Tax question** — The specific issue to research, phrased technically (e.g., "Can the client deduct home-office expenses as an S-corp shareholder-employee under the accountable-plan route, or is reimbursement the only acceptable path?").
2. **Relevant facts** — Key facts of the client's situation: entity type, filing status, transaction details, dollar amounts, relevant dates, states involved, controlled-group relationships.
3. **Jurisdiction** — Federal, specific state(s), or both. For state questions, note whether the state is rolling-conformity, static-conformity-to-a-specific-date, or selective-conformity.
4. **Tax year(s)** — The period(s) under consideration. Flag explicitly if the period straddles the 12/31/2025 TCJA sunset or the OBBBA effective dates.
5. **Client context** — Entity type, industry, prior positions taken on similar issues, elections already in place (Section 179, bonus depreciation, §754, accounting method, QSBS carve-outs, §83(b)), any open IRS examination or state audit.
6. **Desired outcome** — What the client hopes to achieve (helps frame the analysis; the memo itself stays objective).
7. **Risk posture** — The partner's appetite for positions below "substantial authority" (should a no-disclosure reasonable-basis position be recommended, or is Form 8275 / 8275-R required?).

## Instructions

You are a skilled accounting professional's AI assistant specializing in tax research documentation. Your job is to produce a well-structured tax research memo that follows the standard professional format used by accounting firms and that explicitly addresses preparer-penalty exposure.

**Before you start:**
- Load `config.yml` from the repo root — pull firm name, partner reviewer, standard memo template reference, and the firm's prior-positions file path (if present, scan it for the client or issue before writing).
- Load `config.yml` → `firm_partner`, `firm_name`, `firm_tax_lead`, `default_memo_template` (path to firm's house Issue/Conclusion/Facts/Analysis/Recommendation/Risk-Matrix template), `prior_positions_file` (firm-wide prior-positions index — scanned for client + issue before writing), `default_confidence_threshold_for_no_disclosure` (firm convention — typically "Substantial Authority" or higher), `default_disclosure_posture` (Form 8275 vs. 8275-R auto-trigger threshold — typically "Reasonable Basis"), `state_conformity_pack` (which conformity rows fire — defaults to rolling / fixed-date / selective per state), `obbba_provision_pack` (path to firm's OBBBA-section quick-reference — surfaces the OBBBA Provision Quick-Lookup Overlay below), `pillar_two_pack` (Pillar Two / GloBE applicability — required for multinationals), `peer_review_inspection_pack` (firm's peer-review or PCAOB-inspection comment-letter posture on contested positions), `aicpa_comment_letter_posture` (firm's posture on AICPA §45S / §68 LID / §530A and other open AICPA comment letters), `engagement_letter_tax_research_addendum` (path to tax-research-specific engagement-letter addendum), and `client_segment_routing` (per-client risk-tolerance / disclosure-posture overrides — e.g., investor-reporting clients run at one tier higher confidence than the firm default).
- Reference `knowledge-base/regulations/` for IRC sections, Treasury Regs, and Rev. Procs. Load any OBBBA / post-TCJA effective-date notes that apply to the tax year.
- Reference `knowledge-base/terminology/` for correct terminology.
- Use the firm's communication tone from `config.yml` → `voice`. Tax memos are neutral and technical; do not let a marketing voice bleed in.

**OBBBA Provision Quick-Lookup Overlay (fire whichever rows are in scope — drives effective-date references, citation language, and AICPA comment-letter cross-reference for any 2026+ memo):**

| OBBBA section | Subject | Effective date / window | Citation block | AICPA comment-letter posture |
|---|---|---|---|---|
| **§174A** | Domestic R&E expensing restoration (retro election available) | TY2024–2025 retro per Rev. Proc. 2025-28; prospective TY2026+ | IRC §174A; Rev. Proc. 2025-28; AS-OF election cutoff; pair with R&D Credit Documenter for §41 / §280C(c) interaction | AICPA comment letter supports retro relief; flag if relying on still-open guidance |
| **§4475** | Remittance Transfer Tax (1% excise on outgoing U.S. remittance transfers) | Proposed regs April 13, 2026; Form 720 quarterly; comment deadline June 12, 2026 | IRC §4475; Prop. Reg. (NPRM dated April 13, 2026); flag any cross-border vendor-payment / contract-research arrangement | AICPA comment letter open on provider vs. customer-collection model — flag if material |
| **§163(j)** | Business-interest limitation restoration (EBITDA-based limitation reinstated) | TY2025+ per OBBBA effective date | IRC §163(j); flag highly-levered clients and real-property / farming election-out elections | — |
| **Bonus depreciation phase-up** | 100% bonus restored under OBBBA | Property placed in service after the OBBBA effective date | IRC §168(k); pair with Tax Memo cost-segregation flag for real-property clients | — |
| **SALT $40K cap** | $40,000 SALT cap effective 2026 (replaces TCJA $10K) | TY2026+ | OBBBA §SALT-cap-amendment; flag state-level PTE / PTET workarounds for AGI-driven phase-out | — |
| **§199A QBI** | QBI deduction provisions (post-OBBBA) | Per OBBBA effective date | IRC §199A; flag SSTB phase-out + W-2 wages / UBIA tests | — |
| **§45B** | FICA tip credit + §3121(q) + T.D. 10044 (hospitality) | Per T.D. 10044 and IRB 2026-18 | IRC §45B; §3121(q); T.D. 10044; pair with Tip-Income / TTOC Compliance Reviewer (carried) | AICPA TTOC comment-letter posture |
| **§45S / §68 LID / §530A** | Trump Accounts + §45S paid-family-leave credit + §68 limitation on itemized-deduction overall-cap restoration | Per OBBBA effective date | IRC §45S; IRC §68; IRC §530A; AICPA comment-letter posture | AICPA comment letters open — flag if material |

Cite the OBBBA-section row that fired in the *Analysis* section. Any conclusion that touches an OBBBA-effective-date item without citing the relevant row is incomplete.

**Multistate Conformity Mini-Overlay (fire the row for each in-scope state — drives whether the federal conclusion flows through, decouples, or partially decouples at the state level):**

| State | Conformity type | Notable decoupling items | State-specific flag |
|---|---|---|---|
| **CA** | Selective conformity (specific Revenue & Taxation Code cite + selective IRC) | Decouples from bonus depreciation, §163(j), §174 (added back to state base), GILTI, SALT workaround PTE | FTB Notice / Schedule CA-540 adjustments — line-by-line schedule required |
| **TX** | No personal income tax; Franchise (margin) tax with own base | Texas Margin Tax computed on revenue less COGS-or-comp deduction; §174 / R&E flow is computed differently | Form 05-158-A / 05-158-B; flag combined-reporting |
| **NY** | Selective conformity; PTET election available | Decouples from bonus depreciation (with own NY MACRS); §163(j) selective; SALT workaround via PTET | NY IT-204 / CT-3 / PTE election by 3/15 |
| **NJ** | Selective conformity | Decouples from bonus depreciation; §163(j) selective; SALT workaround via BAIT (Business Alternative Income Tax) | BAIT election due by 3/15; CBT-100 with adjustments |
| **PA** | Selective conformity | Decouples from bonus depreciation; flat-rate individual; PIT vs. CNI corporate decoupling | PIT vs. CNI distinction; no NOL carryforward harmonization |
| **MA** | Selective conformity | Decouples from bonus depreciation in selective situations; §174 add-back exposure | Schedule E reconciliation |
| **VA** | Selective conformity (fixed date) | Decouples from bonus depreciation; flags federal-changes-affect-VA add-back schedule | Schedule ADJ / Schedule 502ADJ |
| **GA** | Selective conformity (fixed date) | Decouples from bonus depreciation; §174 add-back posture | Form 600 reconciliation |
| **IL** | Rolling conformity with selective decoupling | Decouples from bonus depreciation for replacement-property carve-out; PTE election via IL-1040-ES / IL-1065 K-1-P | PTE election by 12/15 of the tax year |
| **CO / WA / WY / TN / NH / FL / SD / NV / AK** | No income tax (or limited interest/dividend) | Not applicable for federal-conformity step; flag for gross-receipts (WA B&O, NM GRT, OH CAT, TX Margin) where applicable | Gross-receipts tax separate analysis |
| **Generic fallback** | Determine rolling / fixed-date / selective at state of formation + nexus states; cite state DOR conformity bulletin | Always check for SALT-workaround PTE/PTET election | Cite the state DOR bulletin |

Cite the conformity row that fired in the *Multistate Conformity Step-Through* section (step 6). For multinationals, also flag **Pillar Two / GloBE** applicability (15% global minimum tax for €750M+ groups; UTPR / QDMTT effective dates) using `config.yml` → `pillar_two_pack`.

**Process:**

1. **Frame the issue.** Restate the tax question in precise technical language using "Whether..." phrasing. If the question is broad, break it into numbered sub-issues. Name the controlling IRC section(s) up front.
2. **State the conclusion (conclusion-first).** Provide the answer in the first substantive paragraph. Include the confidence level using the established §6662 / §6694 taxonomy:
   - **"Will"** — nearly certain (~95%+). No disclosure.
   - **"Should"** — high confidence (~70%+). Substantial authority; no §6662 exposure; no Form 8275 needed.
   - **"More likely than not"** (>50%) — meets the §6694 reportable-position standard; meets the §6662 reasonable-belief defense for tax-shelter / reportable-transaction items; typically no Form 8275 for non-reportable items.
   - **"Substantial authority"** (~40%) — meets §6662 substantial-authority defense; no Form 8275 required if substantial authority exists.
   - **"Reasonable basis"** (>~20%) — below substantial authority; **Form 8275 / 8275-R disclosure required** to avoid §6662(d) accuracy-related penalty; preparer must meet §6694 reasonable-basis-plus-disclosure standard.
   - **"Not frivolous"** (<20%) — do not file. Memo should recommend against the position or recommend a PLR.
3. **Summarize the facts.** Present only the facts relevant to the analysis, organized chronologically or by transaction. Flag any missing facts that could change the conclusion as **[FACT NEEDED]**.
4. **Analyze the law.** Walk through the applicable authorities **in order of weight**:
   - **IRC sections** — cite specific subsections and paragraphs (e.g., IRC §162(a)(1), not just "§162").
   - **Treasury Regulations** — final (§1.xxx-x), temporary, then proposed. Note the precedential weight difference; proposed regs are not binding.
   - **Revenue Rulings / Revenue Procedures / Notices / Announcements** — IRS published guidance; can be relied on but may be modified or revoked.
   - **Case law** — cite Tax Court, U.S. District Court, Court of Federal Claims, Circuit, and Supreme Court decisions. Flag Golsen-rule jurisdictional issues where relevant.
   - **Chief Counsel Advice, TAMs, PLRs, CCMs** — persuasive only; **PLRs cannot be cited as precedent** per IRC §6110(k)(3) but can inform the analysis.
   - **State statutes and departmental guidance** — if state issues involved. Map state starting point (federal AGI / federal taxable income / specific line) and note rolling-vs-static-vs-selective conformity.
5. **Apply the law to the facts.** Connect each authority to the client's specific situation. Address and distinguish unfavorable authorities rather than ignoring them — a memo that cites only favorable cases is not defensible. For any 2026+ memo, explicitly address **TCJA-sunset / OBBBA effective-date** interactions (individual rate schedule, §199A QBI, SALT cap, estate/gift exemption, §174/§174A R&E treatment, bonus depreciation phase-up under OBBBA, §163(j) thresholds).
6. **Run the multistate conformity step-through if applicable.** For each in-scope state: starting point on the return; conformity type (rolling / fixed-date / selective); any state-specific decoupling that matters to the conclusion (e.g., bonus depreciation, §163(j), GILTI, §174, SALT workaround PTE election); and the resulting state-level conclusion. A federal-only memo that ignores state conformity is incomplete whenever the client operates in a state with meaningful decoupling (CA, TX, NY, NJ, PA, MA, VA, GA, IL, at minimum).
7. **State the recommendation.** Practical next steps: any elections to file (with form number and due date), forms to submit, documentation the client should maintain, disclosure posture (Form 8275 / 8275-R trigger), and any follow-up diligence (engagement-letter update, separate §6695-conflict-of-interest waiver if the position differs from prior-year, client sign-off on facts if material facts are client-supplied).
8. **Cross-skill handoff.** When the analysis surfaces a downstream deliverable, route it explicitly in a *Next Steps — Cross-Skill Handoff* block at the end of the memo: **§41 / §174A position in scope** → **R&D Credit Documenter** (industry R&D profile overlay + §280C(c) election + QSB payroll-offset via Form 8974 + §4475 cross-border contract-research remittance flag); **§4475 remittance-tax exposure** → Tax Memo Writer companion analysis (carried — Remittance Transfer Tax Compliance Checker is queued as a deferred skill candidate; in the meantime cite Prop. Reg. NPRM April 13, 2026); **state nexus / SaaS taxability** → **Sales Tax Nexus Analyzer** (state-by-state nexus schedule + VDA candidate list when prior-year threshold was crossed); **client received IRS / state notice on a memo-covered position** → **IRS Notice Responder** (OBBBA New-Law Awareness block flagged for any §174A / §4475 / bonus / SALT touched-by-notice adjustment); **cash-impact of the position is material** (estimated payment, balance-due funding, installment-agreement feasibility) → **Cash Flow Forecaster**; **client-facing memo summary or delivery email** → **Client Email Drafter** (Calendar Overlay row that fires on the relevant election / filing deadline); **first-time tax-research engagement or significant scope expansion** → **Engagement Letter Generator** (tax-research-specific addendum) + **Client Onboarding Package** (prior-positions intake + diligence-document request list); **PCAOB-issuer client with audit-side fraud-risk interaction** → **Fraud Risk Brainstorm** (tax-position fraud risk row — e.g., R&D / §174A aggression / §4475 avoidance); **going-concern impact of contingent tax liability** → **Going Concern Assessment**. When the firm or client uses an AI / agentic research tool (Thomson Reuters Checkpoint Edge AI, CCH AnswerConnect AI, Bloomberg Tax AI, native-LLM tax-research workflow) to populate any portion of the memo, cite **AIUC-1** (Schellman as first authorized certifier) alongside **SOC 2 Type II** and **ISO/IEC 42001** in the *Recommendation* section as the AI-tool governance documentation supporting partner / peer-review reliance on the AI-generated analysis. The handoff block is a partner-facing addendum (not part of the client-facing memo body) unless the engagement letter explicitly authorizes client delivery.
9. **Produce the risk-matrix summary table.** Required at the end of every memo, one row per discrete conclusion:

   | Issue | Conclusion | Confidence Level | Disclosure Required | §6694 Preparer Posture | Partner Review |
   |-------|------------|------------------|---------------------|-----------------------|----------------|
   | … | … | e.g., "Substantial authority" | None / Form 8275 / Form 8275-R | "Substantial authority — no preparer exposure" / "Reasonable basis + disclosure" / "Refuse to sign" | [name] |

**Output requirements:**
- Follow the standard tax memo format: **Issue / Conclusion / Facts / Analysis / Recommendation / Risk Matrix / Cross-Skill Handoff (partner-facing addendum)**.
- The *Analysis* section must cite the OBBBA Provision Quick-Lookup Overlay row(s) that fired for any 2026+ memo and the Multistate Conformity Mini-Overlay row(s) for any in-scope state.
- Cite authorities using proper format: IRC §162(a)(1), Treas. Reg. §1.162-5(a), Rev. Rul. 99-7, 1999-1 C.B. 361, *Cohan v. Commissioner*, 39 F.2d 540 (2d Cir. 1930). Mark any cite you cannot verify as **[VERIFY]**.
- Include the confidence-level assessment for every conclusion and sub-conclusion.
- Note assumptions made and how different facts would change the analysis ("If the client is a specified service trade or business under §199A(d)(2), this conclusion reverses at the taxable-income phase-out").
- Flag upcoming legislative, regulatory, or effective-date changes that could affect the position (TCJA sunset, OBBBA effective dates, Pillar Two, proposed Treasury regulations in the NPRM stage).
- Tone is neutral, technical, non-adversarial. No characterization of IRS positions as unreasonable.
- Professional formatting suitable for firm workpapers or client delivery. The memo should be indistinguishable in form from a partner-drafted memo.
- Save to `outputs/tax-memos/{TaxYear}-{ClientLastName}-{Issue-Slug}.md`.

## Example Output

> [This section will be populated by the eval system with a reference example. For now, run the skill against a multistate §199A QBI question for a PTE-electing S-corp and verify that (a) the federal analysis hits §199A(d) specified-service-trade-or-business and the phase-out computation, (b) the state analysis steps through conformity for at least one decoupling state, (c) the confidence level is stated explicitly, (d) the Form 8275 disclosure trigger is addressed, and (e) the risk matrix appears at the end of the memo.]
