---
name: "Client Email Drafter"
category: customer-service
tools: [claude, chatgpt]
difficulty: beginner
time_saved: "~15 min/email"
version: 2.0
last_eval_score: null
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
2. **Client context** — Client name (business or individual), entity type, relationship tone (new client, long-standing client, C-level contact)
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
- Load `config.yml` from the repo root for firm name, contact info, signatory, portal URL, billing rates, and tone
- Reference `knowledge-base/terminology/` for correct industry terms
- Use the firm's communication tone from `config.yml` → `voice`

**Process:**

1. **Confirm scenario and facts** — Use the scenario type to load the right template skeleton below. Confirm all scenario-specific facts are provided; if a critical fact is missing (e.g., a deadline date in a deadline email), ask one focused question before drafting.
2. **Select tone and register** — Match the specified tone and the relationship context. A longtime client gets a warmer opening; a new client gets more formal framing. Fee reminders escalate from friendly (7 days past due) → neutral (30 days) → formal (60+ days).
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

4. **Review for compliance** — Before output:
   - For tax-related emails: no specific dollar advice without documented analysis
   - For fee matters: no statements that could waive the engagement letter's late-fee terms
   - For sensitive data: never include SSN/EIN in the email body; reference "on file" and use the portal for documents
   - Per Circular 230 §10.20 and §7216: do not disclose return information to third parties unless the engagement letter authorizes it

**Output requirements:**
- Subject line (specific, action-oriented, under 10 words)
- Email body formatted for readability (short paragraphs, bullets for lists, bold for deadlines)
- Correct technical language (e.g., "Form 7004" not "business extension," "estimated payment" not "prepayment")
- Always include a specific deadline or next step in the call to action
- Firm signature block pulled from `config.yml`
- No sensitive identifiers (SSN/EIN) in the email body
- If the scenario involves a dollar figure, format as `$1,234.56` consistently
- If helpful, include a short alternative subject line and one-sentence softer/firmer variant
- Saved to `outputs/` if the user confirms

## Example Output

> [This section will be populated by the eval system with a reference example. For now, run the skill with sample input to see output quality.]
