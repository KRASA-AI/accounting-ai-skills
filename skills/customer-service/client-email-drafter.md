---
name: "Client Email Drafter"
category: customer-service
tools: [claude, chatgpt]
difficulty: beginner
time_saved: "~15 min/email"
version: 2.2
last_eval_score: 8.9
---

# ✉️ Client Email Drafter

## Purpose

Draft professional, accounting-specific client emails for the most common firm-to-client scenarios — document requests, deadline reminders, extension notices, notice-response drafts, deliverable delivery, estimated tax reminders, fee reminders, status updates, and K-1 distributions — with the right tone, correct technical language, and complete action items.

## When to Use

Use this skill any time you need to email a client about an active engagement or filing. Especially useful during busy season when volume is high and consistency matters. Works for 1:1 emails and for templated emails sent to a client segment (e.g., all quarterly-estimate clients).

## Required Input

Provide the following:

1. **Email scenario** — Choose one (or describe a custom one):
   - `doc-request` — Ask the client for missing PBC items
   - `deadline-reminder` — Upcoming filing/deposit deadline
   - `extension-notice` — Notice of extension filed (Form 4868/7004)
   - `estimated-tax-reminder` — Quarterly estimate due (Q1 4/15, Q2 6/15, Q3 9/15, Q4 1/15)
   - `notice-response` — Confirming engagement to respond to an IRS/state notice
   - `deliverable-delivery` — Return or financials ready for review/e-signature
   - `k1-distribution` — Delivering K-1s to partners/shareholders
   - `fee-reminder` — Past-due invoice
   - `status-update` — Periodic progress report during an engagement
   - `engagement-renewal` — Annual renewal invitation with new engagement letter
   - `custom` — Describe the purpose
2. **Client context** — Client name (business or individual), entity type, relationship tone (new client, long-standing client, C-level contact), and **client industry / vertical** (SaaS, professional services, retail / e-commerce, construction, restaurant / hospitality, manufacturing, healthcare, nonprofit, dental / vet / optometry, real estate, financial services, agriculture, or generic small business — used to load the vertical-tone profile and the right reference vocabulary)
3. **Scenario-specific facts** — The information specific to the scenario:
   - For `doc-request`: itemized list of missing documents, priority/deadline, portal instructions
   - For `deadline-reminder`: specific form(s), filing deadline, whether an extension is available, what the client needs to do and by when
   - For `extension-notice`: forms extended, new filing deadline, payment still required by original date, estimated balance due if any
   - For `estimated-tax-reminder`: quarter, federal amount, state amount(s), voucher/payment method, safe harbor basis
   - For `notice-response`: notice type (CP2000, Letter 525, state notice, etc.), response deadline, what the firm needs from the client
   - For `deliverable-delivery`: what's attached/in portal, what client needs to review, how to sign (Form 8879 for e-file), refund/balance due
   - For `k1-distribution`: entity, tax year, where K-1 can be retrieved, reminder that the K-1 goes on personal return
   - For `fee-reminder`: invoice number, amount, days past due, payment methods
4. **Tone** — Friendly, neutral, firm, or formal (default: match the tone set in `config.yml` → `voice`)
5. **Urgency** — Normal, time-sensitive (action needed within days), or urgent (same-day)
6. **Call to action** — What the client must do and by when

## Instructions

You are a skilled accounting professional's AI assistant. Your job is to draft an email that is clear, technically correct, action-oriented, and appropriate to the relationship.

**Before you start:**
- Load `config.yml` from the repo root for firm name, contact info, signatory, portal URL, billing rates, and tone. Pull these named keys when present: `firm_name`, `firm_signatory_default`, `firm_contact_phone`, `firm_contact_email`, `portal_url`, `voice`, `vertical_tone_overrides`, `client_segments`, `default_extension_email_balance_due_threshold`, `q_est_safe_harbor_default`, `compliance_calendar_pack`, `partner_draft_notes_format`, `secure_attachment_policy`, `e_signature_provider` (e.g., DocuSign, Adobe Sign, native portal).
- Load `config.yml` → `vertical_tone_overrides` if present (the firm's house list of vertical-specific opening / closing / vocabulary preferences) — this overrides the default vertical-tone profile below
- Load `config.yml` → `client_segments` if present (per-client tone notes, e.g., "always formal," "first-name basis," "decision-maker is the CFO not the founder") — these override the relationship-tone default
- Reference `knowledge-base/terminology/` for correct industry terms
- Reference `knowledge-base/regulations/` for the current-year filing-deadline calendar (federal + state, OBBBA effective-date overlays, FinCEN BOI status, NY LLC Transparency Act effective date)
- Use the firm's communication tone from `config.yml` → `voice`

**Vertical-tone profile defaults (loaded from client industry):**

Resolve the client's industry to its profile *before* drafting. Each profile says (a) what tone register the email opens with, (b) what business cadence the client lives in (so the deadline framing matches their reality), (c) the right reference vocabulary, and (d) the recurring pain points that shape what the client cares about. The profile is the default; if `config.yml` → `vertical_tone_overrides` says otherwise, the override wins.

| Vertical | Tone register | Business cadence | Reference vocabulary | Recurring pain point to acknowledge |
|---|---|---|---|---|
| **SaaS** | Direct, lightly informal, founder-friendly | Quarter-end + ARR-close-driven; month-end is real but quarter-end is the moment | ARR / MRR / NRR / GRR / CAC / CAC payback / burn / runway / Rule of 40 / 409A / RSU vesting / SAFE / preferred stock / cap table / 83(b) | Investor reporting deadlines and runway pressure; deferred-revenue accounting on multi-year contracts |
| **Professional services** | Peer-to-peer professional; assume sophistication | Billable-hour and project-deliverable rhythm; weekly WIP review | Utilization / realization / WIP / unbilled receivables / project margin / write-off / write-down / engagement letter / scope creep | Cash-flow pinch from long collection cycles; partner comp tied to year-end results |
| **Retail / e-commerce** | Approachable, plain English, low jargon | Holiday-driven (BFCM, Q4); inventory cycles and physical counts | Inventory turns / GMROI / shrinkage / COGS / sell-through / SKU / SKU velocity / chargebacks / payment-processor reserve | Inventory financing covenants; sales-tax-nexus complexity across states (link to Sales Tax Nexus Analyzer when relevant) |
| **Construction** | Plain-spoken, no condescension; the client knows their numbers in the field | Project / job basis with progress billing and retainage | WIP report / over-billings / under-billings / retainage / mechanic's lien / cost-plus / lump-sum / change order / percentage-of-completion (POC) / completed-contract / Section 460 / look-back interest | Cash flow during long job cycles; bonding-capacity reporting to sureties; multi-state contractor licensing |
| **Restaurant / hospitality** | Warm, time-respectful (operators are mid-service when they read email) | Daily / weekly cycles; tip reporting and payroll | Prime cost / cost of goods sold % / labor % / occupancy cost / 8027 tip allocation / FICA tip credit / service charge vs. tip / liquor cost | Razor-thin margins; tip-reporting accuracy and §45B FICA tip credit; multi-location compliance |
| **Manufacturing** | Operations-respectful, dollars-per-unit precision | Production calendars and quarter-end cost-accounting closes | Standard cost / variance (volume / price / mix / yield) / WIP / overhead absorption / capitalized labor / §263A UNICAP / R&D credit / §174A | UNICAP and inventory absorption recalculations; export incentives; supply-chain volatility |
| **Healthcare** (medical, dental, vet, optometry) | Clinical-professional; respectful of HIPAA boundaries | Practice-management cycles and benefit-year reset | Production / collections / write-off (insurance vs. courtesy) / contractual adjustment / accounts receivable aging / per-procedure cost / HIPAA / business associate agreement (BAA) | Insurance receivable cycles; equipment §179 / bonus-depreciation timing; partnership / S-corp distributions among providers |
| **Nonprofit** (501(c)(3)) | Mission-respectful, board-ready language | Fiscal-year-end audit calendar; grant cycles | Form 990 / Schedule A public-support test / functional expenses / program-services ratio / restricted vs. unrestricted net assets / endowment / UBIT / donor-advised fund / Schedule O | Grant compliance and donor reporting; functional expense allocation; UBIT exposure on side activities |
| **Real estate** (operators / agents / investors) | Numbers-forward; ROI-aware | Closing-driven; year-end depreciation timing | Cap rate / NOI / DSCR / 1031 / cost segregation / passive activity / §469 / qualified real estate professional / §163(j) / depreciation recapture | Passive-activity loss limits; 1031 timing; cost-segregation studies; entity-structure for liability + tax |
| **Financial services** (RIA, broker-dealer, fund) | Compliance-formal; document-trail aware | Quarter-end performance reporting + audit cycle | Form ADV / surprise custody exam / GIPS / NAV / management fee / carried interest / partnership allocations / §704(c) / §1061 three-year holding / Form PF / SOC 1 reliance | Surprise custody exam; compliance documentation; carried-interest §1061 holding periods |
| **Agriculture / farming** | Plain-spoken, seasonal-aware | Crop / livestock cycle; tax-year ending Dec 31 with March 1 farmer filing rule | §175 soil and water conservation / §180 fertilizer / weather-related sales / income averaging §1301 / Schedule F / depreciation on equipment / farm-loss limitation | Cash-flow swings; March 1 unique filing rule; estate planning around land basis |
| **Generic small business** (default fallback) | Neutral, plainly professional | Calendar-year cycles | Standard tax / accounting terminology only — no vertical-specific jargon | Cash-flow management; on-time tax filings; deduction substantiation |

**Process:**

1. **Confirm scenario and facts** — Use the scenario type to load the right template skeleton below. Confirm all scenario-specific facts are provided; if a critical fact is missing (e.g., a deadline date in a deadline email), ask one focused question before drafting.
2. **Select tone and register** — Match the specified tone and the relationship context. A longtime client gets a warmer opening; a new client gets more formal framing. Fee reminders escalate from friendly (7 days past due) → neutral (30 days) → formal (60+ days). **Apply the vertical-tone profile** for the client's industry: open with a register that fits the vertical (e.g., a SaaS founder gets "quick one for you" energy; a fund administrator gets "documenting for the file" energy), use the right reference vocabulary, and frame deadlines against their business cadence (quarter-end for SaaS, BFCM for retail, March 1 for farmers, project-billing milestones for construction, etc.). When the vertical's recurring pain point is genuinely relevant to the email, acknowledge it in one sentence rather than ignoring it — that single sentence is what separates "templated" from "feels like my CPA gets us."
3. **Draft the email** using the structure for the scenario:

   **Subject line** — Specific and actionable. Good: "Action needed by March 3: 3 remaining tax documents." Bad: "Tax return."

   **Opening** — One sentence of warmth or context (skip if the email is transactional and the relationship is formal).

   **Purpose statement** — One sentence stating why you are writing.

   **Core content** — Scenario-specific. Follow these patterns:
   - **doc-request**: bulleted, itemized list of documents with a brief description of each; state how to deliver (portal, email, drop-off); give a specific deadline.
   - **deadline-reminder**: state the deadline date and the form(s); state what the client needs to do; note consequences of missing the deadline (late-filing penalty, extension option, etc.).
   - **extension-notice**: confirm the extension was filed; state the new filing deadline; **clearly state that extensions extend filing but not payment**; note the estimated balance due and how to pay.
   - **estimated-tax-reminder**: state the quarter and due date; provide federal and state amounts; include payment method (EFTPS, IRS Direct Pay, state portal, voucher by mail); note this is based on safe harbor / prior-year income / current-year projection.
   - **notice-response**: acknowledge the notice by type and notice date; state the response deadline; list documents needed to draft the response; confirm firm's representation.
   - **deliverable-delivery**: state what is ready; state how to access (portal link); list what the client needs to review; describe the e-signature process (Form 8879 for tax); state refund/balance-due amount and payment timing.
   - **k1-distribution**: note the entity and tax year; state where to retrieve; remind the recipient the K-1 amounts flow to their personal return and to share with their preparer.
   - **fee-reminder**: reference the invoice number and date; state the amount and days past due; provide payment methods; escalate tone based on aging.
   - **status-update**: summarize work completed, work in progress, pending items from the client, and next milestone.

   **Call to action** — One clear ask with a specific deadline. Put the deadline in bold.

   **Close** — Brief, matching tone; include a direct phone number for time-sensitive items.

   **Signature** — Name, title, firm name, phone, email, portal URL (from config).

4. **Apply the Regulatory / Deadline Calendar Overlay** before drafting any deadline-bearing email (deadline-reminder, extension-notice, estimated-tax-reminder, notice-response, k1-distribution). The overlay auto-populates the deadline text and the consequence language so the email never names a wrong date.

   | Calendar item | 2026 date | Form / authority | Consequence if missed | Email cadence (when to send) |
   |---|---|---|---|---|
   | Q1 estimated tax | April 15, 2026 | Form 1040-ES (federal) + state vouchers | Underpayment-penalty exposure under §6654 (individuals) / §6655 (corps); safe-harbor analysis applies | T-21 days + T-7 days |
   | Q2 estimated tax | June 15, 2026 | Form 1040-ES + state | Same | T-21 + T-7 |
   | Q3 estimated tax | September 15, 2026 | Form 1040-ES + state | Same | T-21 + T-7 |
   | Q4 estimated tax | January 15, 2027 | Form 1040-ES + state | Same | T-21 + T-7 |
   | Individual 1040 filing | April 15, 2026 | Form 1040 + state | Late-filing penalty 5%/mo to 25%; late-payment penalty 0.5%/mo; interest under §6621 | T-45 + T-21 + T-7 + T-1 |
   | Individual 1040 extension | April 15, 2026 | Form 4868 | **Extension extends filing, not payment** — balance due is still owed by 4/15 | T-21 + T-7; follow-up T-1 if estimated balance due |
   | Individual 1040 extended deadline | October 15, 2026 | Form 1040 | Same as 4/15 if not filed | T-45 + T-21 + T-7 |
   | Partnership / S-corp filing | March 16, 2026 (Mar 15 falls Sun) | Forms 1065 / 1120-S | Late-filing penalty $245/partner-shareholder/mo to 12 months under §6698 / §6699 | T-45 + T-21 + T-7 |
   | Partnership / S-corp extension | March 16, 2026 | Form 7004 | Extension extends filing to Sept 15, 2026 | T-21 + T-7 |
   | C-corp filing (calendar year) | April 15, 2026 | Form 1120 | Late-filing penalty 5%/mo to 25% under §6651 | T-45 + T-21 + T-7 |
   | C-corp extension | April 15, 2026 | Form 7004 | Extension to October 15, 2026 | T-21 + T-7 |
   | Trust / estate (1041) | April 15, 2026 | Form 1041 | Same as 1040 | T-45 + T-21 + T-7 |
   | Form 1041 extension | April 15, 2026 | Form 7004 | Extension to September 30, 2026 (5.5-month extension) | T-21 + T-7 |
   | Form 990 (nonprofit, calendar year) | May 15, 2026 | Form 990 / 990-EZ / 990-N | Late-filing penalty up to $235/day; revocation after 3 consecutive missed years | T-45 + T-21 + T-7 |
   | Form 990 extension | May 15, 2026 | Form 8868 | Extension to November 15, 2026 | T-21 + T-7 |
   | Form 5500 (ERISA — calendar year) | July 31, 2026 | Form 5500 | DOL penalty up to $2,739/day per missed return | T-45 + T-21 + T-7 |
   | Form 5500 extension | July 31, 2026 | Form 5558 | Extension to October 15, 2026 | T-21 + T-7 |
   | FBAR / FinCEN 114 | April 15, 2026 (automatic 6-mo to October 15) | FinCEN 114 | Civil penalty up to greater of $10K or 50% of account balance for willful violation | T-21 + T-7 |
   | FinCEN BOI report (entities formed pre-2024) | Carried per latest FinCEN rule status | FinCEN BOIR | Civil penalty up to $591/day per violation; criminal penalties for willful | T-30 + T-7 if applicable |
   | NY LLC Transparency Act report | January 1, 2026 + ongoing changes within 90 days | NY Dept of State | Suspension of LLC; per-day penalties | T-30 + T-7 |
   | March 1 farmer rule | March 1, 2026 | Form 1040 (if no Q4 estimate paid) | If 2/3 of gross income from farming and no Q4 estimate paid by Jan 15, must file and pay full balance by 3/1 to avoid penalty | T-21 + T-7 |
   | K-1 distribution (1065 / 1120-S filing or extension) | March 16 or September 15, 2026 | Schedule K-1 | Recipient cannot file personal return until K-1 received; communicate timing early | Within 5 business days of filing |
   | OBBBA §174A retro election | Per Rev. Proc. 2025-28 deadlines (small-business cutoff) | Form 3115 / superseding return | Loss of §174A retroactive deductibility for 2022–2024 | T-30 + T-7 from applicable cutoff |
   | OBBBA §4475 Remittance Transfer Tax | Effective dates per final regs (post-June 12, 2026 comment close) | TBD | TBD — flag for partner review | T-21 once final regs issued |
   | SALT $40K cap (OBBBA) | Effective 2026 tax year (income-phase-down) | Schedule A (Form 1040) | Affects Q1 2026 estimated-tax projection | Cover in Q1 estimate email |
   | §45B FICA tip credit / §3121(q) | Ongoing | Form 8846 / 941 | Tip-reporting accuracy affects credit calculation; T.D. 10044 (TTOC) implications | Cover in restaurant / hospitality vertical emails |
   | Generic state notice response | 30 / 60 / 90 days from notice date (state-specific — see **IRS Notice Responder** State Notice Overlay for CA / NY / TX / FL / IL / WA / CO / GA / NJ / PA) | Per state | Notice becomes final; collections / lien risk | Within 5 business days of receipt |

   The overlay row that fires should appear in the email body where deadlines are referenced (e.g., "the federal individual return is due April 15, 2026; the Form 4868 extends filing but not payment, so any balance due is still owed by April 15"). If multiple deadlines apply (e.g., the client is a partnership-member individual who needs both a 1065 K-1 and their own 1040), name both and order by earliest.

5. **Cross-skill handoff block.** When the email scenario implies downstream work, name the companion skill in the partner-facing draft notes (not in the client-facing email body) so the partner knows what to route next:
   - `doc-request` → **Client Onboarding Package** (if first-year client) or **Month-End Checklist** (if recurring monthly-close PBC)
   - `deadline-reminder` involving a tax filing → **Tax Memo Writer** (substantive position) or **Compliance Tracker** (calendar update)
   - `extension-notice` → **Cash Flow Forecaster** (estimated balance-due funding feasibility) if balance is material
   - `estimated-tax-reminder` → **Tax Memo Writer** (if safe-harbor calculation requires written analysis) or **Cash Flow Forecaster** (cash-availability check)
   - `notice-response` → **IRS Notice Responder** (route immediately — apply the OBBBA New-Law Awareness block before drafting any response)
   - `deliverable-delivery` → **Engagement Letter Generator** (if scope-change addendum required) or **Financial Narrative Builder** (if client requests narrative MD&A or board package)
   - `k1-distribution` → none (this is the handoff itself) but flag any K-1 timing risk to the partner
   - `fee-reminder` → **Engagement Letter Generator** (renegotiation if recurring past-due pattern)
   - `status-update` → **Variance Analyzer** (if reporting on month-end variance) or **Month-End Checklist** (if reporting on close progress)
   - `engagement-renewal` → **Engagement Letter Generator** (drives the new engagement letter)
   - Any scenario where the client is a PCAOB-issuer or fiscal periods begin on/after Dec 15, 2026 → reference the PCAOB six-standard wave (QC 1000, AS 1215, AS 2110, AS 2201, AS 1220, AS 2901) in partner draft notes only
   - Any AI-tool-enabled deliverable being communicated to the client → **Engagement Letter Generator** AI-disclosure clause and **AIUC-1 / SOC 2 Type II / ISO/IEC 42001** reference if the firm is asked to substantiate AI-tool governance

6. **Review for compliance** — Before output:
   - For tax-related emails: no specific dollar advice without documented analysis
   - For fee matters: no statements that could waive the engagement letter's late-fee terms
   - For sensitive data: never include SSN/EIN in the email body; reference "on file" and use the portal for documents
   - Per Circular 230 §10.20 and §7216: do not disclose return information to third parties unless the engagement letter authorizes it

**Output requirements:**
- Subject line (specific, action-oriented, under 10 words)
- Email body formatted for readability (short paragraphs, bullets for lists, bold for deadlines)
- Correct technical language (e.g., "Form 7004" not "business extension," "estimated payment" not "prepayment")
- **Vertical-correct vocabulary**: when the client industry is set, use the matching reference vocabulary from the vertical-tone profile (e.g., "your WIP report" for construction, "your latest ARR snapshot" for SaaS, "your Schedule A public-support test" for nonprofit) instead of generic equivalents
- **Vertical-correct cadence framing**: anchor deadlines to the client's natural cycle (quarter-end / BFCM / job-cost milestone / fiscal-year audit / March 1 farmer rule) rather than only to the IRS calendar
- **Regulatory-calendar accuracy**: the deadline named in the email must match the Regulatory / Deadline Calendar Overlay row that fired; if the row is conditional on a state-specific window (notice-response), reference the state-specific row from the **IRS Notice Responder** State Notice Overlay
- Always include a specific deadline or next step in the call to action
- Firm signature block pulled from `config.yml`
- No sensitive identifiers (SSN/EIN) in the email body
- If the scenario involves a dollar figure, format as `$1,234.56` consistently
- If helpful, include a short alternative subject line and one-sentence softer/firmer variant
- **Partner draft notes (separate from the client-facing email body)** when a cross-skill handoff is triggered: a one-line note naming the companion skill(s) so the partner can route follow-up work without re-briefing
- Saved to `outputs/` if the user confirms

## Example Output

> [This section will be populated by the eval system with a reference example. For now, run the skill with sample input to see output quality.]
