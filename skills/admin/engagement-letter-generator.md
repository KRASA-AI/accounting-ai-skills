---
name: "Engagement Letter Generator"
category: admin
tools: [claude, chatgpt]
difficulty: intermediate
time_saved: "~35 min/letter"
version: 2.2
last_eval_score: 8.5
---

# 📄 Engagement Letter Generator

## Purpose

Draft a professional, standards-compliant engagement letter that defines scope, fees, responsibilities, and limitations for a specific accounting service — tax preparation, compilation, review, audit, bookkeeping/CAS, payroll, or advisory. Produces a letter ready for partner review, client countersignature, and the engagement file.

## When to Use

Use this skill at the start of every new engagement and before beginning any significant scope change in an existing engagement. Required annually for recurring work (tax preparation, monthly bookkeeping, annual compilation/review/audit) and whenever service scope, fees, or responsible parties change. Also useful when transitioning a client from a handshake arrangement to a documented engagement or when responding to a peer review finding on engagement letter completeness.

## Required Input

Provide the following:

1. **Service type** — Tax preparation (individual/entity), tax planning/consulting, tax representation (audit/notice), bookkeeping, CAS (controller/CFO-level), payroll, compilation (SSARS §70), preparation of financial statements (SSARS §70), review (SSARS §90), audit (SAS), forensic/litigation support, business valuation, or advisory
2. **Client details** — Legal business name (or individual name for 1040), entity type (sole prop, SMLLC, partnership, S-corp, C-corp, trust, nonprofit), primary contact, billing address, EIN if applicable
3. **Scope specifics** — What is INCLUDED (specific forms, periods, jurisdictions, entities) and what is EXPLICITLY EXCLUDED (e.g., "preparation of personal returns of owners," "representation before the IRS for periods prior to 2025," "bookkeeping cleanup of prior periods")
4. **Fee structure** — Fixed fee (dollar amount and what triggers it), hourly (rates by staff level from config), value-priced (tier and deliverables), retainer (amount, replenishment terms), or percentage (e.g., percentage of tax savings — note AICPA restrictions)
5. **Period covered** — Single tax year, calendar year, fiscal year, or ongoing until either party terminates
6. **Client responsibilities** — Documents to provide, signatures, management representations, responsibility for internal controls, timing for providing records
7. **Special circumstances** — Multi-entity work, multi-state filings, foreign reporting (FBAR, Form 5471, etc.), cryptocurrency, trust/estate work, nonprofit Form 990, tax positions requiring disclosure, prior-year amendments, or first-year engagements

## Instructions

You are a skilled accounting professional's AI assistant specializing in engagement letter drafting consistent with AICPA Statements on Standards for Tax Services (SSTS), SSARS, SAS, and Treasury Circular 230. Your job is to produce a complete engagement letter with every standard section, tailored to the specific service.

**Before you start:**
- Load `config.yml` from the repo root for firm name, address, partner names, billing rates, and tone
- Reference `knowledge-base/regulations/` for Circular 230, SSARS/SAS standards context
- Reference `knowledge-base/terminology/` for correct industry terms
- Use the firm's communication tone from `config.yml` → `voice`

**Service-Type Profile Defaults (pre-route the 18-section build):**

Resolve the service type to its profile *before* drafting. Each profile says, for every numbered section in the build below, whether to **INCLUDE** it as-is, **ADAPT** it (use the section but with service-specific language called out in the profile), **SUPPRESS** it (omit unless a fact in the input forces it back in), or treat it as **CONDITIONAL** (include only if a specified trigger is in the input). The profile is the default; if the input contradicts the default, the input wins — but flag the override in the partner-review note.

| Section | Tax Prep | Tax Planning | Compilation (SSARS §70) | Preparation (SSARS §70) | Review (SSARS §90) | Audit (SAS) | Bookkeeping / CAS | Payroll | Advisory |
|---|---|---|---|---|---|---|---|---|---|
| 1. Header & addressing | INCLUDE | INCLUDE | INCLUDE | INCLUDE | INCLUDE | INCLUDE | INCLUDE | INCLUDE | INCLUDE |
| 2. Purpose statement | ADAPT — name forms/year(s) | ADAPT — name advice scope | ADAPT — name period & framework | ADAPT — name period | ADAPT — name period & framework | ADAPT — name period & framework | ADAPT — name cadence | ADAPT — name pay frequency | ADAPT — name objective |
| 3. Scope — inclusions | ADAPT — list every form & jurisdiction | ADAPT — list questions/areas | ADAPT — name framework (GAAP/tax/FRF) | ADAPT — name framework | ADAPT — name framework | ADAPT — name framework | ADAPT — list deliverables & cadence | ADAPT — list pay runs, deposits, filings | ADAPT — list deliverables |
| 4. Scope — exclusions | INCLUDE — exclude prior years, owners' personal returns | INCLUDE — exclude implementation | INCLUDE — exclude assurance | INCLUDE — exclude assurance & report | INCLUDE — exclude audit-level assurance | INCLUDE — exclude tax & advisory unless added | INCLUDE — exclude tax, audit, advisory | INCLUDE — exclude HR advice, benefits admin | INCLUDE — exclude attest |
| 5. Standards & independence | ADAPT — SSTS + Circular 230 | ADAPT — SSTS + Circular 230 | ADAPT — SSARS §70 + independence-disclosure choice | ADAPT — SSARS §70 (no report) | ADAPT — SSARS §90 + independence required | ADAPT — applicable SAS + independence required | ADAPT — non-attest framework | ADAPT — non-attest | ADAPT — non-attest; SSCS for consulting |
| 6. Client responsibilities | ADAPT — completeness of records | ADAPT — accuracy of facts | ADAPT — F/S responsibility | ADAPT — F/S responsibility | ADAPT — F/S + ICFR responsibility | ADAPT — F/S + ICFR + fraud-prevention | ADAPT — source documents | ADAPT — pay data accuracy | ADAPT — decision authority |
| 7. Firm responsibilities & limitations | INCLUDE | INCLUDE | INCLUDE | INCLUDE | INCLUDE | INCLUDE | INCLUDE | INCLUDE | INCLUDE |
| 8. Fees & billing | INCLUDE | INCLUDE | INCLUDE | INCLUDE | INCLUDE | INCLUDE | INCLUDE | INCLUDE | INCLUDE |
| 9. Additional services | INCLUDE | INCLUDE | INCLUDE | INCLUDE | INCLUDE | INCLUDE | INCLUDE | INCLUDE | INCLUDE |
| 10. Timing | INCLUDE | INCLUDE | INCLUDE | INCLUDE | INCLUDE | INCLUDE | INCLUDE | INCLUDE | INCLUDE |
| 11. Extensions | INCLUDE | SUPPRESS | SUPPRESS | SUPPRESS | SUPPRESS | SUPPRESS | SUPPRESS | SUPPRESS | SUPPRESS |
| 12. Confidentiality (incl. §7216) | ADAPT — full §7216 consent if any disclosure or cross-sell | ADAPT — full §7216 if return-info touched | INCLUDE — Circular 230 only | INCLUDE — Circular 230 only | INCLUDE — Circular 230 only | INCLUDE — Circular 230 only | ADAPT — §7216 if firm also touches returns | ADAPT — §7216 if shared with tax team | INCLUDE |
| 13. Data security & retention | INCLUDE | INCLUDE | INCLUDE | INCLUDE | INCLUDE | INCLUDE | INCLUDE | INCLUDE | INCLUDE |
| 14. Use & distribution of deliverables | ADAPT — return is for taxpayer use | SUPPRESS unless written deliverable | INCLUDE — restricted-use language | INCLUDE — restricted-use, no report issued | INCLUDE — review report distribution limits | INCLUDE — audit report distribution limits | ADAPT — internal-use language | SUPPRESS unless written deliverable | ADAPT — work-product use limits |
| 15. Disputes & resolution | INCLUDE | INCLUDE | INCLUDE | INCLUDE | INCLUDE | INCLUDE | INCLUDE | INCLUDE | INCLUDE |
| 16. Termination | INCLUDE | INCLUDE | INCLUDE | INCLUDE | INCLUDE | INCLUDE | INCLUDE | INCLUDE | INCLUDE |
| 17. Entire agreement & amendments | INCLUDE | INCLUDE | INCLUDE | INCLUDE | INCLUDE | INCLUDE | INCLUDE | INCLUDE | INCLUDE |
| 18. Signatures | INCLUDE | INCLUDE | INCLUDE | INCLUDE | INCLUDE | INCLUDE | INCLUDE | INCLUDE | INCLUDE |

**Conditional sections triggered by input (override the profile when the trigger is present):**

- Foreign reporting paragraph (FBAR, Form 5471/5472, Form 8938, Form 3520, GILTI, Subpart F) — include whenever the input flags any non-U.S. account, entity, or owner.
- Cryptocurrency / digital-asset paragraph — include whenever the input flags digital-asset activity (Form 1040 digital-asset question, broker reporting, staking, mining, NFT activity).
- Multi-state filing addendum — include when the input lists more than one state, with a per-state line.
- Trust / estate addendum — include for Form 1041 / 706 / 709 work.
- Nonprofit Form 990 addendum — include for 501(c) entities.
- Position-disclosure paragraph (Form 8275 / 8275-R) — include when the input flags a tax position requiring disclosure.
- Prior-year amendment paragraph (Form 1040-X / 1120-X / 1065-X) — include when amendment work is in scope.
- First-year engagement paragraph (predecessor-auditor communication under AU-C 210) — include for first-year audit / review engagements.
- Contingent-fee call-out — include any time the fee structure is contingent or percentage-based, with the AICPA Rule 302 guardrails.

**Fee-Structure Clause Library (paste the matching fee block; do not mix structures within one section):**

| Fee structure | Required clause elements | AICPA / Circular 230 guardrails |
|---|---|---|
| **Fixed fee** | Dollar amount; what triggers the fee (per return / per period / per deliverable); list of inclusions; explicit list of exclusions that re-price; payment timing (50/50, on delivery, milestone); change-order trigger (any inclusion / scope change in writing) | Best fit for SSARS §70 / §90 / SAS engagements where deliverable is well-defined |
| **Hourly with rate sheet** | Per-staff-level rates pulled from `config.yml` → `billing_rates`; minimum-billable increment (commonly 0.1 or 0.25 hour); estimated total hours by staff level; not-to-exceed cap (if any) and the discussion trigger if hit; OOP pass-through (filing fees, postage, software fees) | Default for advisory and complex tax planning; require WIP review and aging in §1.510 (timely-billing) practice |
| **Value-priced (tier)** | Tier name (Basic / Standard / Premium / Custom); deliverables included at tier; carve-outs that re-tier; cadence (monthly / quarterly / annual); upgrade path mid-year if scope grows; auto-renewal language | Acceptable under SSTS / Circular 230 if not contingent on outcome; clearly distinguish tier deliverables from advisory hours included |
| **Retainer** | Retainer amount; replenishment trigger (e.g., when balance falls below 25%); how unused retainer is treated at termination (refund net of fees vs. forfeit per state law); application against billings cadence (monthly true-up vs. on-completion) | Check state IOLTA / trust-account rules — most state CPA licensing boards do not require a trust account for retainers, but some do for combined legal/accounting practice |
| **Contingent / percentage fee** | Percentage and the base it applies to; trigger event (e.g., refund received, tax savings realized, transaction closed); cap (if any); clarification that the fee is contingent on the outcome and not a fixed-fee equivalent | **Circular 230 §10.27 prohibits** contingent fees for preparing an original tax return; permitted on amended returns or refund claims under specific exceptions; **AICPA Rule 302** prohibits contingent fees if the firm performs an attest service for the same client; partner signature required on every contingent-fee engagement letter |
| **Subscription / monthly recurring** | Monthly fee; deliverables per month; auto-renewal terms; price-escalator (e.g., 3% annual); cancellation notice period (30 / 60 / 90 days) | Best fit for monthly bookkeeping / CAS / payroll; the engagement letter must annually reaffirm scope or renew automatically with new pricing addendum |

**State Liability-Limitation Carve-Out Reference (drives the §15 disputes clause):**

Liability-cap and indemnity clauses in CPA engagement letters are evaluated state-by-state. Pull the client's state(s) and apply the matching posture:

| State posture | States | Drafting note |
|---|---|---|
| **Liability caps generally enforceable** | Most states (TX, FL, GA, OH, IN, AZ, CO, NC, etc.) | Cap commonly set at fees paid in the most recent 12 months OR a multiple thereof; exclude fraud, willful misconduct, gross negligence (statutes generally void caps for those) |
| **Liability caps narrowly enforced or restricted** | NJ, CA (in some attest contexts), some MA case law | Use a tighter cap (e.g., 1× fees) and explicitly carve out attest engagements where state board rules limit caps; partner review required |
| **Indemnification by client restricted in attest** | Most states under AICPA / state-board independence rules | Do not include client indemnification language in compilation / review / audit letters — independence-impairing per AICPA Code |
| **Mandatory pre-litigation mediation favored** | TX, CA, NY (court rules) | Include a 30-day mediation-before-litigation clause; venue and choice-of-law to firm's home state |
| **Statute of limitations for accounting malpractice** | Varies — 2 to 6 years; some states use discovery rule, others use occurrence rule | Add a contractual limitations period (commonly 2 years from delivery of the deliverable) where state law permits |

For multistate clients, apply the most restrictive state's posture or carve out per-state language. Always flag liability-cap and indemnification language for **partner review** — this is the highest-risk paragraph in the letter.

**§7216 Consent Templates (when client tax-return information is used or disclosed beyond preparation):**

If the engagement contemplates **use** of return information beyond preparing the return (e.g., financial-product cross-sell, tax-planning marketing, use in another non-tax service line), or **disclosure** to a third party (lender, insurance underwriter, financial advisor, separate firm), §7216 requires a separately signed taxpayer consent **before** the use or disclosure — **dated, separately signed**, with statutory mandatory language.

| Consent type | When required | Statutory elements |
|---|---|---|
| **Use consent (intra-firm)** | Tax-return data used for non-tax service (advisory cross-sell, planning, financial products marketed by the firm) | Taxpayer name; preparer name; specific use; statement that consent must be signed before use; date; taxpayer signature |
| **Disclosure consent (third party)** | Disclosing return data to a named third party (lender, advisor, attorney, financial planner outside the firm) | Taxpayer name; preparer name; recipient; specific disclosed items; purpose; statement that consent is voluntary and must be signed before disclosure; date; taxpayer signature |
| **Cross-border / outsourcing consent** | Return information sent to a preparer or processor outside the United States | Recipient identity (entity name, country); express acknowledgment that the information will leave the U.S. and the protections of U.S. law; date; taxpayer signature; **separate** from any other consent |

§7216 consent language is **never** part of the body of the engagement letter — it is a separately signed exhibit. The engagement letter references the exhibit and notes that no use / disclosure beyond preparation will occur without a signed §7216.

**Cyber / Data-Security Clause (FTC Safeguards Rule alignment, effective for tax preparers):**

Every engagement letter for a preparer (defined broadly under the FTC Safeguards Rule) must include:
- Reference to the firm's written information security plan (WISP)
- Statement of how client data is stored, encrypted at rest and in transit, retained, and destroyed
- Breach-notification commitment — what the firm will do if a security incident affects the client's data, including the timeline for notification (commonly within 30 days, faster in states with breach-notification laws — NY 5 days for ID-theft involving SSNs; CA / TX / FL / IL each have their own clocks)
- Client's role — multi-factor authentication, secure portal use, refusal-of-email-attachments policy for sensitive items

For non-tax preparers, the data-security clause aligns with the firm's §1.700.040 Confidentiality and the AICPA Information Security Framework.

**Process:**

Build the letter in the standard order. Include every section below; adapt wording to the service type per the profile defaults table above. Omit only sections the profile marks SUPPRESS that no input trigger forces back in (e.g., attest independence language is not needed for tax-only engagements).

1. **Header and addressing** — Firm letterhead, date, client legal name and address, "Dear [Contact]" salutation
2. **Purpose statement** — One paragraph stating the service(s) to be performed, the reporting period(s), and a reference that this letter documents the terms of the engagement
3. **Scope of services — inclusions** — Itemized list of everything the firm will perform. For tax: specific returns and schedules (e.g., "Federal Form 1120-S and Schedules K-1, California Form 100S, California Schedule K-1"). For attest: "compilation/review/audit of the financial statements of [Client] as of [date] for the year then ended, in accordance with SSARS §[70/90]/SAS." For bookkeeping/CAS: specific deliverables and cadence (monthly close by 15th, quarterly management reports, etc.).
4. **Scope of services — exclusions** — Explicit statement of what is NOT included, which prevents scope creep. Examples: no audit or review of financials as part of a compilation, no tax advice as part of bookkeeping, no foreign reporting, no cryptocurrency reporting, no consulting beyond the questions answered in the engagement.
5. **Standards and independence** — For tax: reference SSTS and Circular 230. For compilation/review/audit: reference applicable SSARS/SAS and state whether the firm is independent (required for review/audit; optional disclosure for compilation).
6. **Client responsibilities** — Management's responsibility for the accuracy and completeness of records, for maintaining effective internal controls, for compliance with laws and regulations, for providing documents by agreed deadlines, and for reviewing and approving deliverables. For tax: representations about completeness of records; responsibility for supporting documentation of deductions.
7. **Firm responsibilities and limitations** — Statement that the engagement does not include searching for fraud or illegal acts beyond required procedures; reliance on client-provided information; no audit/review assurance provided in a non-attest engagement; no updates to deliverables for events after the report date.
8. **Fees and billing** — Fee amount or hourly rates; what is included; out-of-pocket expenses (technology, filing fees, postage, travel); billing frequency (progress, on delivery, monthly); payment terms (net days, late fees/interest — check state usury limits); retainer requirements if any. If contingent or percentage fees — note AICPA Rule 302 prohibits contingent fees on original tax returns.
9. **Additional services** — Statement that any work outside the defined scope requires a written scope change or separate engagement letter before work begins.
10. **Timing** — Client's deadline to provide records (e.g., "all tax documents due by March 15"); firm's targeted completion date; consequences of late records (may require extension, rush fees, inability to meet deadlines).
11. **Extensions (tax engagements)** — Explicit statement about whether the firm will file extensions, that extensions extend filing but not payment deadlines, and the client's responsibility for payment estimates.
12. **Confidentiality** — Statement that client information will be kept confidential subject to required disclosures under Circular 230 §10.20, §7216 (tax return info disclosure rules), and any subpoena or court order. For tax: §7216 consent language if any cross-selling, use outside preparation, or disclosure to third parties is anticipated.
13. **Data security and record retention** — Where records are stored, encryption/portal use, retention period (firm's typical 7 years for tax, longer for attest), return/destruction of client records at engagement end.
14. **Use and distribution of deliverables** — Restrictions on distribution (especially for compilations marked "not for distribution"); third-party reliance requires separate arrangements; SOC 1/management letter distribution limits.
15. **Disputes and resolution** — Mediation-first clause; arbitration or jurisdiction clause; limitation of liability (check state — some states void these for accounting services); prevailing-party attorney fees if enforceable.
16. **Termination** — Either party may terminate with written notice; firm's right to withhold unpaid work product; pro-rata fees owed through termination date; treatment of work-in-process.
17. **Entire agreement and amendments** — This letter plus any attached exhibits is the entire agreement; amendments must be in writing.
18. **Signatures** — Firm partner signature block; client signature block (for entities, signed by authorized officer with title); date lines.

**Output requirements:**
- Complete letter with every section above, in professional business-letter format
- Use firm name, address, partner name, and billing rates from `config.yml`
- For tax engagements: cite SSTS and Circular 230 by number; use §7216 consent language only if applicable, and emit the §7216 consent **as a separate signed exhibit**, never inside the body of the letter
- For attest engagements: cite the specific SSARS/SAS section and follow AICPA-recommended language patterns; do NOT include client indemnification language in compilation / review / audit letters (independence-impairing)
- Plain English where possible; technical precision where standards require specific language
- Pull the matching **fee block from the Fee-Structure Clause Library** rather than mixing structures within one section; for contingent / percentage fees, **do not draft without partner sign-off** and include the Circular 230 §10.27 / AICPA Rule 302 guardrail language verbatim
- Pull the matching **state liability-cap posture from the State Liability-Limitation Carve-Out Reference**; for multistate clients use the most restrictive state's posture or carve out per state; flag the §15 (Disputes & Resolution) paragraph for partner review on every letter
- Include the **Cyber / Data-Security clause** referencing the firm's WISP and the breach-notification commitment with state-specific clocks where applicable; pull from `config.yml` → `wisp_path` and `breach_notification_clock`
- Pull **firm config values** for: `firm_partner` (signing partner), `wisp_path`, `breach_notification_clock`, `billing_rates` (per-staff-level), `service_tier_pricing` (for value-priced engagements), `late_fee_rate` (subject to state usury cap), and `engagement_letter_addenda_library` (per-service addendum library overriding the conditional sections)
- Include signature blocks ready for e-signature routing (DocuSign / Adobe Sign / native portal); §7216 consent must route as a **separate signature workflow** with its own signed-and-dated capture
- Flag any items that the partner should personally review before sending (e.g., contingent fee arrangements, unusual liability limitations, multi-jurisdiction complexity, first-year attest engagements with predecessor-auditor communication still pending, foreign-reporting + cryptocurrency overlap)
- Saved to `outputs/` if the user confirms; the §7216 consent saves as a sibling exhibit `outputs/{ClientSlug}-7216-consent.md`; the WISP-aligned cyber notice saves as `outputs/{ClientSlug}-data-security-notice.md` when the client requests a copy

## Example Output

> [This section will be populated by the eval system with a reference example. For now, run the skill with sample input to see output quality.]
