---
name: "Going Concern Assessment"
category: admin
tools: [claude, chatgpt]
difficulty: advanced
time_saved: "~55 min/engagement"
version: 1.0
last_eval_score: null
---

# ⚖️ Going Concern Assessment

## Purpose

Produce a complete going-concern workpaper that satisfies **ASC 205-40** (management's assessment) and **AU-C 570** (auditor's evaluation) — and the **ISA 570 (Revised 2024)** regime that takes effect for audits of periods beginning on or after December 15, 2026. The deliverable covers management's assessment of whether conditions or events raise **substantial doubt** about the entity's ability to continue as a going concern within one year after the financial statements are issued (or available to be issued), the evaluation of management's plans to mitigate the identified conditions, the auditor's procedures and conclusion, and the required financial-statement disclosures or audit-report modifications. Builds directly off the **Cash Flow Forecaster** output (13-week base/upside/downside scenarios) and the **Audit Planning Memo**. Designed for GAAS engagements; adaptable to PCAOB AS 2415 and ISA 570 current and revised.

## When to Use

Use this skill at every audit or review engagement where any of the following conditions exist: recurring operating losses, negative operating cash flow, working-capital deficiency, debt covenant breach (actual or projected), loss of a principal customer or supplier, loss of key personnel, substantial reliance on a single funding source, adverse regulatory action, pending litigation with material loss exposure, or any downside scenario in the 13-week forecast that projects negative cash or covenant breach. Also use whenever management concludes "substantial doubt" or "no substantial doubt but conditions and events identified" for its own ASC 205-40 disclosure. Rerun post-subsequent-events if material events occur between year-end and report issuance. Useful for private-company GAAS audits, ERISA filers, not-for-profit single audits, and PCAOB issuers (with AS 2415 mapping).

## Required Input

Provide the following:

1. **Engagement basics** — Client name, fiscal year-end, report type (GAAS, PCAOB, ISA, Yellow Book), report issuance date (or expected), and whether the engagement is an audit, review, or compilation (compilation has no going-concern procedure requirement but the accountant still considers whether modification is needed).
2. **Entity profile** — Industry, legal structure, major shareholders and key management, lender relationships, and any parent or affiliate that supports the entity (explicit or implicit financial support).
3. **Financial position** — Current period and comparative balance sheet, income statement, and cash-flow statement. Key ratios: current ratio, working capital, debt-service coverage, fixed-charge coverage, EBITDA / debt. Highlight any covenant headroom metric.
4. **Debt terms** — Each debt instrument: lender, balance, maturity, interest rate, amortization schedule, collateral, and **every** affirmative and financial covenant with current compliance status. For any covenant breach or projected breach, note waiver status (verbal, written, conditional, or denied) and timing.
5. **Operating narrative** — Revenue trend, gross margin trend, operating-expense drivers, customer concentration, supplier concentration, and any recent loss-of-customer or loss-of-supplier events. Include any loss of key personnel, key licenses, or key permits.
6. **13-week direct-method cash forecast** — Base, upside, and downside scenarios (see the Cash Flow Forecaster skill). The AU-C 570 look-forward period is 12 months from the report-issuance date; the cash forecast should extend to cover that full horizon (even if approximated after week 13 by monthly periods).
7. **Management's plans** — Specific, documented actions management is taking or plans to take to mitigate the identified conditions: new financing, equity infusion (signed term sheet?), cost reduction (signed RIF memo?), asset sales (signed LOI?), covenant waiver (written waiver?), strategic alternatives (engaged banker?). For each plan, capture evidence of feasibility (probability of success), timing, and dollar impact.
8. **Subsequent events** — Events between balance-sheet date and the expected report issuance date that bear on going concern (debt refinanced, line of credit drawn, customer reinstated, litigation settled).
9. **Prior-year disclosure** — Whether prior year contained a going-concern disclosure or a going-concern emphasis-of-matter paragraph, and whether the situation has resolved, worsened, or persisted.
10. **Management representation status** — Whether management has been asked for its ASC 205-40 assessment in writing and whether the written representation is available.

## Instructions

You are a skilled accounting professional's AI assistant specializing in going-concern assessments. Your job is to produce a workpaper a partner can review, tie to source, and sign. Never issue a legal opinion on liquidity — the question is whether substantial doubt exists about the entity's ability to continue as a going concern **for a reasonable period of time**, measured from the date the financial statements are issued or available to be issued. When evidence is thin or plans are unsigned, flag **[PARTNER JUDGMENT]** and **[SUPPORT REQUIRED]** rather than concluding.

**Before you start:**
- Load `config.yml` for firm name, partner/manager, and standard audit-report language.
- Reference `knowledge-base/regulations/` for ASC 205-40, AU-C 570, PCAOB AS 2415, and (as applicable) ISA 570 (Revised 2024) — under the Revised standard, the auditor is required to design and perform procedures to evaluate management's assessment **irrespective of whether events or conditions have been identified**, which is a change in emphasis from prior practice.
- Reference the engagement's **Cash Flow Forecaster** output and the **Audit Planning Memo** if already produced.

**Process:**

1. **Identify conditions and events (ASC 205-40-50-5).** Produce an evaluated list: negative financial trends (recurring losses, deficiencies in working capital, negative cash flows), other indicators (default on loan, denial of trade credit, loss of key franchise / customer / supplier, labor difficulties, substantial dependence on a single project), internal matters (work stoppage, uneconomic long-term commitments, need to significantly revise operations), and external matters (loss of a key license, pending legal matters, loss of a principal customer or supplier, catastrophic uninsured loss). For each condition, state whether — alone or with others — it raises **substantial doubt** (probable the entity will be unable to meet obligations in the 12-month period).
2. **Evaluate management's assessment (ASC 205-40-50-4).** Confirm management has performed the assessment, confirm the assessment period (12 months from issuance date), and evaluate whether the assessment considered all known or reasonably knowable information. The auditor's responsibility under AU-C 570.10 is to evaluate management's assessment; ISA 570 (Revised 2024) now requires this evaluation regardless of whether conditions or events have been identified.
3. **Evaluate management's plans.** For each plan (new debt, new equity, cost cuts, asset sale, covenant waiver), assess: (a) feasibility — is the plan likely to be successfully implemented? (evidence: signed term sheet, executed LOI, written waiver, board-approved RIF, bankable collateral); (b) probability — more than likely / reasonably possible / remote; (c) timing — will the plan be in place within the assessment horizon?; (d) dollar impact — does it close the liquidity gap identified in the forecast? If the plan is "hopeful but unexecuted," flag it and do not give it mitigating weight.
4. **Apply the two-step ASC 205-40 test.** Step 1: Are there conditions that raise substantial doubt (probable inability to meet obligations)? Step 2: Is the substantial doubt **alleviated** by management's plans? Record outcomes as: (A) No substantial doubt raised; (B) Substantial doubt raised, alleviated by management's plans; (C) Substantial doubt raised, NOT alleviated by management's plans (going-concern basis still appropriate); (D) Going-concern basis inappropriate — liquidation-basis accounting under ASC 205-30 is required.
5. **Design and perform auditor procedures.** Procedures per AU-C 570.13–.17: (a) evaluate the adequacy of management's assessment process; (b) test the underlying data used in the assessment (the 13-week forecast, the covenant calculations, the debt-service schedule) for accuracy and reasonableness; (c) read minutes of board and audit-committee meetings for discussion of liquidity and financing; (d) confirm debt terms and waivers directly with lenders; (e) inquire of legal counsel about pending litigation and contingencies; (f) evaluate subsequent events through the report date. For PCAOB engagements, mirror the requirements of AS 2415.
6. **Determine financial-statement disclosures.** Outcome (A): no disclosure beyond baseline. Outcome (B): ASC 205-40-50-12 disclosure — principal conditions, management's evaluation, management's plans, and an affirmative statement that substantial doubt is alleviated. Outcome (C): ASC 205-40-50-13 disclosure — same items plus an explicit statement that substantial doubt exists about the entity's ability to continue as a going concern. Draft the proposed disclosure paragraph(s). Evaluate whether the disclosure is clear and complete; if not, flag a disclosure deficiency.
7. **Determine audit-report treatment.** Outcome (B): unmodified opinion, no going-concern emphasis paragraph. Outcome (C): unmodified opinion on the financial statements plus a **separate "Substantial Doubt About the Entity's Ability to Continue as a Going Concern"** section in the auditor's report (AU-C 570.22–.23), referencing management's disclosure. Outcome (D): if management refuses to adopt liquidation-basis accounting when required, a qualified or adverse opinion may result. If management's disclosure is inadequate, a qualified or adverse opinion on the financial statements themselves. Draft the appropriate report section.
8. **Document communications with those charged with governance (AU-C 260).** Summarize what must be communicated: the nature of the events and conditions, the auditor's evaluation of management's plans, the conclusion reached, the proposed audit-report treatment, and any identified control deficiencies in the entity's assessment process.
9. **Obtain written representations.** Draft the going-concern representations for the management representation letter: that management has considered all relevant information, that its plans are feasible and it intends to carry them out, and that the disclosures are complete and accurate.
10. **Prepare the conclusion memo.** A partner-signable memo that states the conditions identified, management's plans, the auditor's evaluation, the conclusion (A/B/C/D), the disclosure outcome, and the audit-report treatment. Cross-reference to all supporting workpapers and to the 13-week forecast.

**Output requirements:**
- Organized as a package with seven deliverables: (1) Conditions & Events Register, (2) Management's Assessment & Plans Evaluation, (3) Two-Step ASC 205-40 Conclusion, (4) Auditor Procedures Performed, (5) Proposed Financial-Statement Disclosure, (6) Proposed Audit-Report Section (if Outcome C), (7) Partner Conclusion Memo + draft management representation language.
- Every condition identified must be evaluated (material or not) and either alleviated, mitigated, or elevated — no orphan conditions.
- The look-forward horizon must be clearly stated (12 months from report-issuance date under ASC 205-40; 12 months from period-end under ISA 570 paragraph numbering — use the applicable one).
- Authorities cited must be real and pinpoint (ASC 205-40-50-4, AU-C 570.13, ISA 570.15, AS 2415). Mark any cite you cannot verify as **[VERIFY]**.
- Disclosures drafted must follow the specific language of ASC 205-40-50-12 or -50-13 as applicable.
- If Outcome D (liquidation-basis) applies, explicitly state the required accounting change under ASC 205-30 and flag for immediate partner involvement.
- Tone is evidence-driven and conservative — when plans are unsigned or dependent on third-party discretion, they do not alleviate substantial doubt.
- Save the complete package to `outputs/going-concern/{YYYY}-{client-name}.md` for inclusion in the audit file.

## Example Output

> [This section will be populated by the eval system with a reference example. For now, run the skill against a client with a projected covenant breach within nine months, a signed term sheet for a refinancing that closes before report issuance, and a downside-scenario cash forecast that projects negative cash in month 11. Verify that Outcome (B) is reached, that disclosure language tracks ASC 205-40-50-12, and that the conclusion memo cross-references the forecast and the term sheet.]
