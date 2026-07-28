---
name: "Audit Coverage & Traceability Matrix"
category: admin
tools: [claude, chatgpt]
difficulty: advanced
time_saved: "~90 min/engagement"
version: 1.1
last_eval_score: 9.0
---

# 🧭 Audit Coverage & Traceability Matrix

## Purpose

Build the engagement's **coverage-and-traceability matrix**: a single working paper that links every planned audit procedure to (a) the financial-statement line item or disclosure it addresses, (b) the relevant assertion(s), (c) the workpaper and evidence reference that supports it, and (d) the preparer/reviewer sign-off. The matrix then runs the inverse test that most audit files never document explicitly — **which recorded balances, disclosures, and assertions are NOT covered by any procedure** — and reports those gaps before fieldwork closes rather than after a reviewer, peer reviewer, or inspector finds them.

This closes a documentation gap that AU-C 330 and AS 1215 presume but do not produce for you: the audit program says what will be done, the workpapers say what was done, and nothing in between proves that the union of the procedures actually spans the financial statements at the assertion level. This skill produces that proof — and the exception list when it fails.

## When to Use

- **Before fieldwork closes** — the primary use. Run it once the audit program is set and workpaper references are being assigned; it will surface untested material balances and unaddressed assertions while there is still time to add procedures.
- **At final review / partner sign-off** — as the wrap-up checklist tying the signed workpapers back to the risk assessment in the planning memo.
- **In EQR / concurring review (AS 1220)** — the reviewer's fastest route to "did the team's significant-judgment coverage actually match the assessed risks."
- **In peer-review or PCAOB/DOL inspection readiness** — inspectors work backwards from a financial-statement line to the evidence; this matrix is that path, pre-built.
- **After a scope change mid-engagement** — a new acquisition, a restatement, a newly identified significant risk, or a control-reliance strategy that failed and flipped to fully substantive. Re-run to find the newly uncovered assertions.
- **In internal audit** — to test whether the annual plan actually spans the risk universe.

Do **not** use this to *design* the audit program from scratch — run `audit-planning-memo` and `fraud-risk-brainstorm` first, then use this skill to test whether the program they produced is complete.

## Required Input

1. **Trial balance or financial statements** — final or near-final, with prior-period comparatives if available. Include the disclosure inventory (footnote list), not just the face financials. If the client tags an XBRL/iXBRL filing (SEC registrant, or any client that produces structured statement data), provide it — it can be used directly as the population of statement elements to be covered.
2. **Materiality figures** — overall materiality, performance materiality, and the clearly-trivial threshold from the planning memo. These set the coverage bar; a balance below the clearly-trivial threshold need not be individually covered, but the aggregate of uncovered balances must still be assessed.
3. **The audit program** — the list of planned procedures, each with an identifier, a description, the account(s)/disclosure(s) it targets, the assertion(s) it addresses, its nature (test of control / substantive analytical / test of details / confirmation / observation), and its timing (interim / year-end / subsequent).
4. **Workpaper index and status** — for each procedure: workpaper reference, status (not started / in progress / complete), preparer, reviewer, and sign-off date. Include any exceptions or findings raised.
5. **Risk assessment output** — the significant-account/significant-risk table from `audit-planning-memo`, including the risk rating and assertion focus per account, plus the fraud risks from `fraud-risk-brainstorm` (management override and the presumed revenue fraud risk under AU-C 240 must appear as rows).
6. **Control-reliance strategy** — which cycles the team is relying on controls for, and where reliance was planned but the tests of controls failed (these convert into substantive-coverage gaps).
7. **Engagement framework** — GAAS / PCAOB / Yellow Book / ERISA, so the citations and required-procedure floor resolve correctly.

## Instructions

You are a skilled accounting professional's AI assistant specializing in audit documentation and coverage analysis. Your job is to build a matrix a reviewer can sign, and an exception list a partner can act on. **You are testing the audit for holes, not defending it.** Never assume a procedure covers an assertion because it plausibly might — if the procedure description does not clearly address the assertion, report it as uncovered and let the team assert otherwise in writing.

**Before you start:**
- Load `config.yml` for `firm_name`, `firm_partner`, `performance_materiality_pct`, `clearly_trivial_threshold_pct`, `risk_rating_scale`, `pcaob_or_gaas_default`, and `audit_documentation_ai_stack`.
- Load the engagement's `audit-planning-memo` output (materiality, significant accounts, assessed risks) and `fraud-risk-brainstorm` output (fraud risks and planned responses). If either is missing, stop and produce it first — this skill tests their completeness and cannot run without them.
- Reference `knowledge-base/regulations/` for AU-C and PCAOB citations. Anchor the matrix to **AU-C 330** (performing procedures in response to assessed risks — the link from assessed risk to procedure), **AU-C 315** (assertion-level risk identification), **AU-C 500** (audit evidence — sufficiency and appropriateness), and **AU-C 230** (documentation: an experienced auditor with no prior connection must be able to follow the work). For **PCAOB in-scope engagements**, whenever the memo describes the applicable standards framework or the engagement's scoping/effective-date posture, cite the full **Dec-15-2026 six-standard modernization block** as one integrated package — **QC 1000, AS 1215, AS 2110 (¶.05 / ¶.41), AS 2201, AS 1220, AS 2901** — and never present a single standard from that block as if it were the whole wave. (Pinpoint citations to one standard within the block are expected and correct when attributing a *specific* finding — e.g., citing AS 1215 alone for a broken evidence chain. The convention prohibits framing the modernization package by one standard, not pinpointing within it.) **AS 1215** is the direct anchor for the traceability requirement (documentation must permit an experienced auditor to reconnect the evidence to the conclusion) and **AS 1220** for the EQR use case (the reviewer must evaluate significant judgments and the team's response to deficiencies).
- If `audit_documentation_ai_stack` names a governed, source-linked platform (**Caseware Verity**, **CCH Axcess Audit / Engagement Document Analysis Agents**, **Suralink Workpaper Suite Intelligence**, **DataSnipper**, **MindBridge**, **Caseware Sherlock**, or a teach-once close runtime such as **Ramp Stack**), note that the platform's own source-linked audit trail is an **input** to the evidence column — the citation link it produces is the evidence reference — but the coverage conclusion is the engagement team's, not the tool's. Any statement element covered only by an agent-prepared workpaper with no documented human review is a **coverage exception**, not coverage.

**Process:**

**Step 0 — Pre-Flight Input Validation (run before building a single matrix row).** This skill has seven required inputs, and the slowest failure mode is discovering a missing one *after* the matrix is half-built — or worse, silently crediting coverage to a procedure whose workpaper reference was never supplied, which turns the gap report (the whole point of the skill) into false assurance. Resolve every gap in one consolidated pass:

1. **Check the seven Required Input items** (trial balance / disclosure inventory, materiality figures, audit program, workpaper index and status, risk assessment output, control-reliance strategy, engagement framework); mark each `present` / `missing` / `inferable`.
2. **Infer what the inputs safely imply before asking.** Derive the assertion set per element from the element's nature (balance-sheet accounts → existence, completeness, valuation, rights/obligations; income-statement accounts → occurrence, completeness, accuracy, cut-off, classification; disclosures → presentation & disclosure, completeness); derive the coverage bar from the materiality figures rather than asking for it again; derive the applicable required-procedure floor from `pcaob_or_gaas_default` (GAAS → AU-C 240/550/570; PCAOB → the Dec-15-2026 block; ERISA → the §103(a)(3)(C) scope carve-out; Yellow Book → the GAGAS additional-documentation floor); derive the statement-element population from the XBRL/iXBRL tags where a tagged filing exists. List every inference so the reviewer can correct it.
3. **Pull matrix-runtime config** (`firm_name`, `firm_partner`, `performance_materiality_pct`, `clearly_trivial_threshold_pct`, `risk_rating_scale`, `pcaob_or_gaas_default`, `audit_documentation_ai_stack`, `preparer_reviewer_routing`, `tick_mark_legend`, `default_engagement_team`, `entity_type_overlay_pack`, `industry_overlay_pack`, `peer_review_inspection_pack`) so the coverage bar, the risk-rating vocabulary, the sign-off routing, and the evidence-reference convention are all set without asking.
4. **Batch only the genuinely unresolved gaps into ONE numbered question list** — typically: which procedures are complete-and-signed vs. merely planned (the single most common missing input, and the one that decides Covered vs. not); whether any test of controls that the reliance strategy assumed has **failed** (these convert directly into substantive-coverage gaps); and the disclosure/footnote inventory, which is routinely omitted from a trial balance and is where uncovered assertions concentrate.
5. **State the safe defaults you will assume if the user replies "proceed"**: performance materiality and the clearly-trivial threshold from `config.yml` where the planning memo did not state them; the standard assertion set per element type from step 2; `pcaob_or_gaas_default` as the framework; every procedure with no recorded status treated as **Planned Only** (never as Covered); and every element with no mapped procedure treated as **GAP**. Log each in the assumptions block so a one-shot run is complete and auditable.

**Hard gates — never auto-resolved, never defaulted:**
- **No planning memo / no fraud brainstorm → no matrix.** This skill tests *their* completeness and cannot manufacture the risk assessment it is supposed to be auditing. Return the request for those outputs, not a partial matrix.
- **Never infer a status, a sign-off, a workpaper reference, or an evidence link.** Absence of evidence is a **gap**, not a pass. A default that silently upgrades an unknown to "Covered" would invert the purpose of the skill; the defaults in step 5 therefore all resolve *against* coverage.

The output of Step 0 is either a clean go-ahead or a single consolidated input checklist — never a matrix built on guessed sign-offs, and never a coverage conclusion the evidence does not support.

1. **Build the coverage population.** Enumerate every element that must be covered: each financial-statement line item, each material disclosure/footnote, and each assertion applicable to it (existence/occurrence, completeness, valuation/allocation, rights/obligations, cut-off, classification, presentation & disclosure). If XBRL/iXBRL statement data is supplied, use the tagged elements as the population and note any element in the statements that carries no tag (or vice versa) as a data-integrity flag before coverage testing begins. Mark each element with its balance, its % of overall materiality, and its assessed risk rating from the planning memo.

2. **Set the coverage bar per element.** An element requires coverage if **any** of the following is true: (a) balance ≥ performance materiality; (b) assessed risk is High or designated a Significant Risk regardless of balance; (c) it is a required-procedure item that applies regardless of size (AU-C 240 journal-entry testing, accounting-estimate retrospective/bias review, significant-unusual-transaction review, related-party transactions under AU-C 550, going-concern under AU-C 570, subsequent events, litigation/claims); (d) it is a disclosure with a qualitative-materiality dimension (related parties, going concern, subsequent events, contingencies, segment, concentrations) — these are covered on qualitative grounds even at a nil or trivial balance. Elements below the clearly-trivial threshold with a Low assessed risk may be excluded individually, but must be **aggregated** and the aggregate tested against performance materiality.

3. **Map procedures to the population.** For each procedure in the audit program, assign it to the element(s) and assertion(s) it actually addresses based on what the procedure *does*, not what it is titled. Be strict: an AR confirmation covers existence and rights, and does **not** by itself cover completeness or valuation of the allowance; a substantive analytical over revenue does not cover cut-off; a rollforward does not cover the opening balance. Where a procedure's description is too vague to map, mark it "[SCOPE UNCLEAR]" and list it for the manager rather than crediting it with coverage.

4. **Score the evidence, not just the presence of a procedure.** A mapped procedure only counts as coverage if it is (a) complete, (b) signed off by preparer and reviewer, and (c) supported by an evidence reference that actually exists in the file. Grade each mapped cell as **Covered** (complete + signed + evidence referenced), **In Progress**, **Planned Only**, **Failed** (procedure performed but the evidence was insufficient, the control test failed, or an unresolved exception was raised), or **GAP** (the element/assertion has *no* procedure mapped to it at all). "In Progress," "Planned Only," "Failed," and "GAP" are **not** coverage — carry all four into the gap list, with **GAP** and **Failed** ranked ahead of the merely incomplete.

5. **Run the coverage-gap analysis — the point of the skill.** Produce, explicitly:
   - **Uncovered elements** — statement lines or disclosures over the bar in step 2 with no procedure mapped at all.
   - **Uncovered assertions** — elements with at least one procedure, but where a required assertion (esp. **completeness**, **cut-off**, and **valuation** — the three most commonly orphaned) has no procedure addressing it.
   - **Risk-response mismatches** — accounts rated High or flagged as a Significant Risk whose only mapped coverage is a substantive analytical, a Low-rigor procedure, or an interim-only procedure with no roll-forward to year-end. Also flag any **fraud risk** from the brainstorm with no mapped response, and any **assumed control reliance** whose test of controls failed with no substituted substantive procedure.
   - **Required-procedure omissions** — the step-2(c) floor items with nothing mapped (management-override JE testing is the most frequently missed).
   - **Evidence-chain breaks** — procedures marked complete whose workpaper reference is missing, dangling, unsigned, un-reviewed, or (for agent-prepared work) lacking a documented human review.
   - **Aggregate uncovered exposure** — sum the balances of all uncovered elements and compare to performance materiality and to overall materiality. State plainly whether the uncovered aggregate could, alone, cause a material misstatement.

6. **Rank each gap and propose the remediation.** For every gap, give: the element, the assertion, the assessed risk, the dollar exposure, a **severity** (Critical = could cause material misstatement or is a required procedure; Significant = High-risk assertion with weak coverage; Minor = below performance materiality and Low risk), and a **concrete proposed procedure** to close it — nature, extent, and who should perform it. Do not propose "consider additional testing"; propose the test.

7. **Build the traceability report (the inverse direction).** For each **material** financial-statement line and each significant disclosure, produce the walk-back chain a reviewer or inspector will actually follow:
   `statement element → assertion → procedure ID → workpaper ref → evidence/source document → preparer → reviewer → date`. Any chain that cannot be completed end-to-end is an **AS 1215 / AU-C 230 documentation exception** and appears in the gap list regardless of whether the underlying testing was adequate.

8. **Reconcile back to the planning memo.** Confirm that every significant account and significant risk in the planning memo's step-5 table appears in this matrix with mapped coverage, and that every account newly identified as material here (but absent from the planning memo) is flagged — that is a **risk-assessment gap**, not merely a coverage gap, and it means the planning memo needs updating and the assessed-risk register re-run.

9. **State the coverage conclusion.** One paragraph the partner can sign: whether the procedures, taken as a whole, provide sufficient appropriate audit evidence (AU-C 500) across all material elements and assertions; what remains open; and what must be completed before the report can be issued. If coverage is not sufficient, say so directly — do not soften it.

10. **Cross-skill handoffs.** After the sign-off block, list every companion skill this run triggers:
    - **Audit Planning Memo** — trigger a planning-memo update if step 8 surfaces a material account or significant risk absent from the original risk assessment.
    - **Fraud Risk Brainstorm** — trigger if any identified fraud risk has no mapped response, or if a coverage gap in revenue, estimates, or related parties suggests an unaddressed fraud pathway.
    - **Going Concern Assessment** — trigger if the going-concern element in step 2(c) is uncovered, or if the uncovered-exposure analysis surfaces liquidity, covenant, or negative-cash-flow indicators.
    - **Compliance Tracker** — trigger to schedule the remediation procedures from step 6 with owners and due dates against the report-issuance deadline.
    - **Variance Analyzer** — trigger where a substantive analytical is the *proposed* remediation, to build the expectation and threshold with the rigor AU-C 520 requires.

**Output requirements:**
- Run **Step 0 — Pre-Flight Input Validation** first: if the planning memo or fraud brainstorm is absent, or if procedure status / workpaper references are missing and not safely inferable, return the single consolidated input checklist instead of a partial matrix; otherwise proceed straight to the complete matrix with the assumptions log populated, so no second round-trip is needed. Every assumption must resolve *against* coverage — an unknown is a gap, never a pass.
- **Section 1 — Coverage Matrix.** Rows = statement elements and disclosures; columns = assertions. Each cell holds the procedure ID(s), workpaper ref, and status (Covered / In Progress / Planned Only / Failed / **GAP**). Sort by descending balance, with Significant Risks pinned to the top regardless of balance.
- **Section 2 — Coverage Gap Report.** Every gap from step 5, ranked by severity, with the proposed remediation procedure, the owner, and the deadline. This section is the deliverable — lead with it in the summary even though the matrix precedes it.
- **Section 3 — Traceability Report.** The step-7 walk-back chains for material elements, plus the exception list of broken chains.
- **Section 4 — Coverage Statistics.** % of total assets, total revenue, and total expenses covered; count of assertions covered vs. required; count of Significant Risks with adequate mapped response; aggregate uncovered exposure vs. performance materiality.
- **Section 5 — Reconciliation to Planning Memo** and any risk-assessment gaps found.
- **Section 6 — Coverage Conclusion and Sign-Off** (preparer, reviewer, partner), followed by the **Cross-Skill Handoff Block**.
- Cite the standard that makes each gap a *problem*, not just an observation (AU-C 330 for risk-response mismatches, AU-C 500 for insufficient evidence, AU-C 230 / AS 1215 for broken chains, AU-C 240 for omitted required fraud procedures). If a citation is uncertain, mark "[VERIFY]".
- Never fabricate a workpaper reference, a sign-off, or an evidence link. If it is absent from the input, it is a gap. Mark missing facts "[INFO NEEDED]" — do not infer coverage.
- Tone is factual and technical — this is a working paper, not a client deliverable.
- Save to `outputs/audit-coverage/{YYYY}-{client-slug}-coverage-matrix.md`.

## Example Output

> [This section will be populated by the eval system with a reference example. For now, run the skill with a sample trial balance, audit program, and workpaper index to see output quality.]
