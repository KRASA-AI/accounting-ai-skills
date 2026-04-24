---
name: "Tax Memo Writer"
category: operations
tools: [claude, chatgpt]
difficulty: advanced
time_saved: "~45 min/memo"
version: 2.1
last_eval_score: 8.3
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
- Reference `knowledge-base/regulations/` for IRC sections, Treasury Regs, and Rev. Procs. Load any OBBBA / post-TCJA effective-date notes that apply to the tax year.
- Reference `knowledge-base/terminology/` for correct terminology.
- Use the firm's communication tone from `config.yml` → `voice`. Tax memos are neutral and technical; do not let a marketing voice bleed in.

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
8. **Produce the risk-matrix summary table.** Required at the end of every memo, one row per discrete conclusion:

   | Issue | Conclusion | Confidence Level | Disclosure Required | §6694 Preparer Posture | Partner Review |
   |-------|------------|------------------|---------------------|-----------------------|----------------|
   | … | … | e.g., "Substantial authority" | None / Form 8275 / Form 8275-R | "Substantial authority — no preparer exposure" / "Reasonable basis + disclosure" / "Refuse to sign" | [name] |

**Output requirements:**
- Follow the standard tax memo format: **Issue / Conclusion / Facts / Analysis / Recommendation / Risk Matrix**.
- Cite authorities using proper format: IRC §162(a)(1), Treas. Reg. §1.162-5(a), Rev. Rul. 99-7, 1999-1 C.B. 361, *Cohan v. Commissioner*, 39 F.2d 540 (2d Cir. 1930). Mark any cite you cannot verify as **[VERIFY]**.
- Include the confidence-level assessment for every conclusion and sub-conclusion.
- Note assumptions made and how different facts would change the analysis ("If the client is a specified service trade or business under §199A(d)(2), this conclusion reverses at the taxable-income phase-out").
- Flag upcoming legislative, regulatory, or effective-date changes that could affect the position (TCJA sunset, OBBBA effective dates, Pillar Two, proposed Treasury regulations in the NPRM stage).
- Tone is neutral, technical, non-adversarial. No characterization of IRS positions as unreasonable.
- Professional formatting suitable for firm workpapers or client delivery. The memo should be indistinguishable in form from a partner-drafted memo.
- Save to `outputs/tax-memos/{TaxYear}-{ClientLastName}-{Issue-Slug}.md`.

## Example Output

> [This section will be populated by the eval system with a reference example. For now, run the skill against a multistate §199A QBI question for a PTE-electing S-corp and verify that (a) the federal analysis hits §199A(d) specified-service-trade-or-business and the phase-out computation, (b) the state analysis steps through conformity for at least one decoupling state, (c) the confidence level is stated explicitly, (d) the Form 8275 disclosure trigger is addressed, and (e) the risk matrix appears at the end of the memo.]
