---
name: "Engagement Letter Generator"
category: admin
tools: [claude, chatgpt]
difficulty: intermediate
time_saved: "~30 min/letter"
version: 2.0
last_eval_score: null
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

**Process:**

Build the letter in the standard order. Include every section below; adapt wording to the service type. Omit only sections that are clearly inapplicable (e.g., attest independence language is not needed for tax-only engagements).

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
- For tax engagements: cite SSTS and Circular 230 by number; use §7216 consent language only if applicable
- For attest engagements: cite the specific SSARS/SAS section and follow AICPA-recommended language patterns
- Plain English where possible; technical precision where standards require specific language
- Clearly distinguish fixed-fee vs. hourly vs. value-priced fee language (don't mix)
- Include signature blocks ready for e-signature routing
- Flag any items that the partner should personally review before sending (e.g., contingent fee arrangements, unusual liability limitations, multi-jurisdiction complexity)
- Saved to `outputs/` if the user confirms

## Example Output

> [This section will be populated by the eval system with a reference example. For now, run the skill with sample input to see output quality.]
