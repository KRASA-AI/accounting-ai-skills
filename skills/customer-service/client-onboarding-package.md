---
name: "Client Onboarding Package"
category: customer-service
tools: [claude, chatgpt]
difficulty: beginner
time_saved: "~35 min/client"
version: 2.1
last_eval_score: 9.0
---

# 📋 Client Onboarding Package

## Purpose

Generate a complete, partner-signable new-client onboarding package including a welcome letter, **service-type × entity-type document request matrix**, **30/60/90-day service timeline**, key contact sheet, **per-software technology setup playbook**, **industry overlay** for industry-specific PBC items, **first-year-engagement risk-management items** (conflict check, AML/sanctions screen, AU-C 210 predecessor-auditor communication, §6695 conflict-of-interest waiver), and an FAQ tuned to the engaged service. Designed so the new client receives one polished package, the firm has every diligence item documented for the engagement file, and the next-task assignments to staff are explicit.

## When to Use

Use this skill when signing a new client to any service — tax preparation, bookkeeping / CAS, audit, review, compilation, payroll, or advisory. Especially useful when transitioning a client from another firm (where AU-C 210 predecessor-auditor communication is required for first-year attest engagements, and a "prior-year deliverables" reconstruction step is usually warranted). Also useful when an existing client adds a materially new service line — e.g., a tax-only client adding monthly CAS, a bookkeeping client adding audit — because the document-request, timeline, and tech-setup deliverables are all service-specific.

## Required Input

Provide the following:

1. **Client details** — Business name, entity type (sole prop, SMLLC, partnership / multi-member LLC, S-corp, C-corp, trust / estate, nonprofit), **industry / vertical** (SaaS, professional services, retail / e-commerce, construction, restaurant / hospitality, manufacturing, healthcare / dental / vet / optometry, nonprofit, real estate, financial services, agriculture, generic small business), state(s) of operation, primary contact name and role, billing contact (if different)
2. **Services engaged** — Tax preparation (1040 / 1120 / 1120-S / 1065 / 990 / 1041), tax planning / consulting, tax representation (notice / audit), monthly bookkeeping, CAS / fractional-CFO, payroll, compilation (SSARS §70), preparation (SSARS §70), review (SSARS §90), audit (SAS), advisory, forensic / litigation support, business valuation
3. **Fiscal year-end** — Calendar year or specific fiscal year-end date; flag short-period or stub-year engagements
4. **Accounting software** — What the client currently uses (QuickBooks Online, QuickBooks Desktop, Xero, Sage Intacct, NetSuite, Sage 50, FreshBooks, Wave, spreadsheets) or what the firm will set up. Include payroll provider (Gusto, ADP, Paychex, Rippling, in-house) and any operational systems (POS, e-commerce, billing, time-tracking) that need a feed into the GL
5. **Key deadlines** — Upcoming filing or reporting deadlines relevant to the engagement (next return, next quarterly estimate, next sales-tax filing, next payroll deposit, next compilation / review / audit issuance date, next lender / investor reporting date)
6. **Special circumstances** — Prior-year issues (late filings, missing returns, IRS / state notices, prior-period adjustments), entity changes (new state registration, S-election effective date, new EIN, recent M&A, BOI filing status), first-year-engagement flags (predecessor auditor / accountant — AU-C 210 communication required for first-year attest), engagements requiring a §7216 consent
7. **First-year-engagement metadata** — Was there a predecessor firm? If so, name and contact for the AU-C 210 communication; access to prior workpapers permitted? Any open disagreements with the predecessor? Any risk-of-material-misstatement concerns surfaced during acceptance?
8. **Engagement economics** — Service tier (basic / standard / premium / custom), fixed fee or hourly with rate sheet from `config.yml`, retainer required (Y / N and amount), value-pricing tier if applicable

## Instructions

You are a skilled accounting professional's AI assistant. Your job is to create a polished, thorough onboarding package that makes new clients feel organized and confident from day one — and that doubles as the firm's risk-management record for the engagement file.

**Before you start:**

- Load `config.yml` from the repo root for firm name, partner of record, billing rates, default response-time SLAs, and tone
- Load `config.yml` → `firm_partner` (the partner of record), `engagement_lead` (the person responsible for the onboarding), `client_success_lead` (the firm-side primary point of contact), `default_response_sla` (e.g., 24 business hours for client-portal messages), `portal_url` (the firm's secure document-sharing portal), `document_intake_method` (portal / encrypted email / drop-off), `default_engagement_team` (the staff stack: senior, staff, admin), and `onboarding_kit_template` (the firm's house template path, when present)
- Load `config.yml` → `vertical_overrides` if present (firm house list of vertical-specific PBC items, FAQ snippets, or tech-setup steps that override the defaults below) and `service_tier_pricing` (the firm's published tier definitions)
- Load `config.yml` → `firm_intuit_partner_tier` (the firm's Intuit Partner Program tier — Select / Premier / Elite — which governs IES / IAS provisioning rights, discount eligibility, and dedicated-support routing), `firm_qboa_to_ias_migration_status` (the firm's posture on the QuickBooks Online Accountant → Intuit Accountant Suite transition — `on-qboa` / `migrating` / `on-ias` — which determines whether the tech-setup playbook references QBOA or IAS provisioning flows), `client_segment_routing` (the firm's segment map — e.g., small-business / mid-market / construction-vertical — used to pick the ledger-backbone row and to route the cross-skill handoff), and `aiuc1_disclosure_default` (whether the firm discloses AI / agentic tooling certifications in client-facing onboarding materials by default)
- Reference `knowledge-base/terminology/` for correct industry terms
- Reference `knowledge-base/regulations/` for AU-C 210 (initial audit engagements — predecessor auditor communication), Circular 230 §10.20, §7216 consent rules, AICPA Code of Conduct §1.295 (independence for engagement onboarding), and the FTC Safeguards Rule (effective for tax preparers — written information security plan required)
- Use the firm's communication tone from `config.yml` → `voice`

**Service-Type × Entity-Type Document Matrix (build the document request from this matrix, not from a generic checklist):**

Resolve the engagement to a column (service type) and a row (entity type). Each cell lists the PBC items the client must provide. Then layer on the industry-overlay items and any conditional items triggered by the input.

| Item / Service | Tax Prep | Tax Planning | Bookkeeping / CAS | Payroll | Compilation (SSARS §70) | Review (SSARS §90) | Audit (SAS) | Advisory |
|---|---|---|---|---|---|---|---|---|
| **Prior tax returns (3 years)** | Required | Required | Useful | n/a | Useful | Required | Required | Useful |
| **Prior financial statements (3 years)** | Useful | Useful | Required | n/a | Required | Required | Required | Required |
| **Prior auditor / accountant contact (AU-C 210)** | Useful | Useful | Useful | n/a | Useful | **Required for first-year** | **Required for first-year** | n/a |
| **Bank statements (12 months)** | Required | Useful | Required | n/a | Required | Required | Required | Useful |
| **Credit card statements (12 months)** | Required | Useful | Required | n/a | Required | Required | Required | Useful |
| **Chart of accounts** | Useful | Useful | Required | n/a | Required | Required | Required | Useful |
| **Trial balance (current period)** | Required | Useful | Required | n/a | Required | Required | Required | Useful |
| **General ledger detail (period)** | Useful | Useful | Required | n/a | Useful | Required | Required | Useful |
| **W-2s / 1099s issued and received** | Required | Required | Useful | Required | Useful | Useful | Required | Useful |
| **K-1s (received and to be issued)** | Required (PTE) | Required | Useful | n/a | Useful | Useful | Useful | Useful |
| **Estimated tax payment record** | Required | Required | Useful | n/a | n/a | Useful | Useful | Useful |
| **Fixed asset register / depreciation schedule** | Required | Useful | Required | n/a | Required | Required | Required | Useful |
| **Loan agreements + amortization schedules** | Required | Useful | Required | n/a | Required | Required | Required | Required |
| **Lease agreements (operating + finance, ASC 842)** | Useful | Useful | Required | n/a | Required | Required | Required | Useful |
| **Significant contracts (top 10 customer / vendor)** | Useful | Useful | Useful | n/a | Useful | Required | Required | Required |
| **Board / member minutes (last 24 months)** | n/a | Useful | Useful | n/a | Useful | Required | Required | Useful |
| **Articles, bylaws, operating agreement** | Required | Required | Useful | n/a | Required | Required | Required | Useful |
| **EIN letter (CP 575) + state registrations** | Required | Required | Required | Required | Required | Required | Required | Required |
| **Form 2848 / 8821 (POA / TIA)** | Required | Useful | n/a | n/a | n/a | n/a | n/a | n/a |
| **§7216 consent (if any cross-sell / disclosure)** | Conditional | Conditional | Conditional | Conditional | n/a | n/a | n/a | Conditional |
| **AR aging / AP aging** | Useful | Useful | Required | n/a | Required | Required | Required | Required |
| **Inventory listing / count sheets** | Useful (TPP) | Useful | Required (if inv.) | n/a | Required | Required | Required | Useful |
| **Payroll registers (4 quarters)** | Required (employer) | Useful | Required | Required | Required | Required | Required | Useful |
| **941 / 940 / state UC returns** | Required (employer) | Useful | Required | Required | Required | Required | Required | Useful |
| **Sales-tax filings (last 12 months)** | Required (sellers) | Useful | Required | n/a | Required | Required | Required | Useful |
| **Insurance policies (D&O, GL, cyber, fiduciary)** | n/a | Useful | Useful | n/a | Required | Required | Required | Useful |
| **Prior-period IRS / state notices** | Required | Required | Useful | Useful | Useful | Required | Required | Useful |
| **Conflict-check screen (firm-side)** | Required | Required | Required | Required | Required | Required | Required | Required |
| **AML / sanctions screen (OFAC)** | Required | Required | Required | Required | Required | Required | Required | Required |

Conditional items triggered by the input (override the matrix when the trigger is present):

- **Foreign reporting** (FBAR / 5471 / 5472 / 8938 / 3520 / GILTI) — list foreign accounts, foreign entities, and foreign-source income
- **Cryptocurrency / digital asset** — list wallet addresses, exchange-statement export, NFT activity, staking / mining records
- **Multistate filings** — registration certificates and prior-year apportionment workpapers per state
- **Cost segregation / 1031 exchange (real estate)** — prior cost-seg study, 1031 exchange documentation, replacement-property identification
- **Trust / estate work** — trust agreement, prior 1041s, beneficiary K-1s, §645 / §663(b) elections, basis schedule
- **Nonprofit (501(c)(3))** — Form 1023 / 1024, latest determination letter, board policies (conflict, whistleblower, document-retention), donor-restriction documentation
- **R&D credit (§41 / §174A)** — project inventory, time-tracking export, contract-research SOWs, supply invoices, prior Form 6765 + Section G if previously claimed
- **Sales-tax nexus exposure** — sales-by-state report (24 months), marketplace-facilitator settlement reports, FBA inventory-state report
- **First-year-audit / review** — predecessor-auditor permission letter, prior workpapers access, opening-balance verification documents
- **PTET election** — per-state election filings and payment confirmations

**Per-Software Technology Setup Playbook (use the row matching the client's actual software stack):**

| Software | User invitation flow | Bank-feed connection | Document-share / portal | Closed-period lock | Backup / export |
|---|---|---|---|---|---|
| **QuickBooks Online** | Gear → Manage Users → Add user → Accountant → enter firm email → grants accountant tools (close books, journal entries, reports) | Banking → Link Account; for read-only feeds use `Pulse` or `LiveFlow`; for >5,000 tx clients use `Transaction Pro` | QBO Documents tab + firm portal (drag-and-drop into client folder); enable Online Banking attachments | Settings → Advanced → Close the books → set date + password | Reports → Custom Reports → export GL / TB / JE Detail to CSV monthly |
| **QuickBooks Desktop** | Multi-User mode → Set up Users → External Accountant; firm uses Accountant's Copy + Dividing Date | Bank Feeds Center → Set up account; .QBO file import as fallback | Send Accountant's Copy to firm via secure portal; firm returns .QBY | Edit → Preferences → Accounting → Set Date / Password to lock prior periods | File → Backup Company → Local + offsite (firm portal) |
| **Intuit Accountant Suite (IAS)** *(QBOA successor — use this row when `firm_qboa_to_ias_migration_status` = `on-ias` or `migrating`)* | IAS Team → Add client / Add team member → assign firm role; client books provisioned from inside IAS (firms on QBOA migrating to IAS retain accountant tools — close books, journal entries, reclass, reports); confirm `firm_intuit_partner_tier` provisioning rights | Banking → Link Account (Plaid / bank-direct); firm-managed feeds inherited at provisioning | IAS document workspace + firm portal; request-list / PBC tracker native to IAS | Books → Close the books → date + password (per client) | Reports → export GL / TB / JE Detail; IAS batch-export across the client roster monthly |
| **Intuit Enterprise Suite (IES)** *(mid-market backbone — use this row for mid-market clients per `client_segment_routing`)* | IES Admin → Users / Roles → assign "External Accountant" role, per-entity and per-dimension; multi-entity consolidation provisioned at firm tier | Banking → Bank Feeds (multi-entity); statement import fallback for unsupported banks | IES document store per entity / dimension + firm portal for source docs the GL doesn't hold | Period Lock per book (GAAP / tax / management), per entity | Reports → multi-entity GL / TB / consolidations; saved to firm portal |
| **IES Construction Edition** *(Intuit's industry-specific ERP — use for construction-vertical clients; forces job-cost dimension coding)* | IES Admin → Users / Roles → "External Accountant" with job-cost + WIP-equity permissions; confirm construction-vertical override is enabled so every fixed-asset, COGS, and direct-labor line carries a job-cost dimension | Banking → Bank Feeds; job-cost-aware COA maps feeds to cost codes | IES document store + firm portal; certified-payroll and change-order docs attached at the job level | Period Lock per book + WIP-equity reconciliation lock | Reports → WIP / over-under-billings, job-cost GL, TB; saved to firm portal |
| **Xero** | Settings → Users → Invite User → Adviser role (full org access including reports + payroll + bank rules) | Bank Accounts → Add Bank Account; Xero direct feeds via Plaid or bank-direct; CSV import fallback | Files inbox + firm portal + email-to-files alias `inbox@orgID.xerofiles.com` | Advisor → Year End → Set Lock Date | Reports → Export → All Journals + GL + TB to CSV / PDF monthly |
| **Sage Intacct** | Company → Admin → Users → Roles & Permissions → assign "External Accountant" role; permissions per-entity | Bank → Bank Feed; supports Plaid + bank-direct; statement import fallback | Documents → Upload + assign to entity / dimension; firm portal for source docs the GL doesn't store | Period Lock → set per book (GAAP / tax / management) | Reports → Custom → GL / TB / Subledgers; saved to firm portal |
| **NetSuite** | Setup → Users / Roles → New role "External CPA" with custom permissions; assign to firm partner / staff | Banking → Bank Feeds (Yodlee); CSV statement import for unsupported banks | File Cabinet folder per client + firm portal; record-level attachments | Setup → Accounting → Manage Accounting Periods → close period | Saved Search → export GL / TB / AR aging / AP aging |
| **Sage 50** | Maintain → Users → External Accountant role; .ptb backup file send to firm | Direct Connect via Plaid; OFX import fallback | Documents tab; firm portal for ancillary | Tasks → Settings → Closed Periods | File → Backup → portal upload monthly |
| **FreshBooks** | Settings → Team → Invite Accountant (free seat) | Banking → Connect Bank; CSV import fallback for unsupported banks | FreshBooks Documents + firm portal | Reports → period reports for read-only review (FreshBooks lacks formal close) | Reports → Export → GL / TB / Profit & Loss CSV |
| **Wave / spreadsheets** | n/a — firm receives bank statements + receipts via portal | Statement import only | Firm portal is the system of record | n/a — firm maintains the ledger | n/a — firm owns the export |

For each software, also document:
- **MFA enforcement** — confirm the client has MFA enabled on the accounting platform; if not, schedule that as a Day 1 task
- **API / integration map** — list every system that writes to the GL (POS, e-commerce, billing, payroll, expense management, AP automation, time tracking) and confirm read access for the firm
- **FTC Safeguards Rule alignment** — for tax preparers, document the firm's written information security plan and the client's role in the data-flow

**AIUC-1 conditional citation block (include when `aiuc1_disclosure_default` is `on` and the firm's onboarding stack uses AI / agentic tooling that touches client data):** When the onboarding or downstream bookkeeping stack relies on AI / agentic tooling — Intuit Enterprise Suite / IAS agentic categorization, Xero JAX auto-categorization, QBO Intuit Assist, AP-automation or document-extraction layers (Vic.ai / Booke.ai), or close / reconciliation agents (FloQast Visual Agent Builder, Suralink Workpaper Suite Intelligence) — add a one-paragraph data-governance note to the client-facing package citing the underlying tool certifications: **AIUC-1** (Schellman as the first authorized certifier) alongside **SOC 2 Type II** and **ISO/IEC 42001** (AI management system standard). For PCAOB-registered audit work, reference **PCAOB AS 1215** (electronic / AI-tool working-paper documentation; effective Dec. 15, 2026). The note states that the firm retains full professional responsibility and that AI tooling operates human-in-the-loop — it is not the preparer or auditor of record.

**Industry overlay (add the rows that match the client's vertical to the document checklist and the FAQ):**

| Vertical | Industry-specific PBC items | FAQ additions |
|---|---|---|
| **SaaS** | ARR / MRR snapshot, deferred-revenue waterfall + contract terms (multi-year), ASC 606 SSP analysis, 409A valuation, cap table + SAFE / convertible-note schedule, §174A R&E expenditure detail, Stripe / billing platform exports | "How do you handle deferred revenue for our annual contracts?" "Do we qualify for the §41 R&D credit?" "How does §174A interact with our R&E spend?" |
| **Construction** | WIP report (current + prior 12 months), backlog schedule, certified-payroll reports (Davis-Bacon), surety bond limits + workpaper, retainage schedule, change-order log, prevailing-wage compliance | "How do you handle WIP and over/under billings?" "Can you support our surety reporting?" "Do you do certified payroll?" |
| **Restaurant / hospitality** | POS daily-sales export, Form 8027 detail (large F&B), tip log, liquor-license renewal date, health-permit renewal date, merchant-processor settlement reports | "Do you compute the §45B FICA tip credit?" "How do you reconcile our POS to QBO?" "Can you handle our 8027?" |
| **Retail / e-commerce** | Sales-by-state report (24 months), marketplace-facilitator settlement reports, FBA inventory-state report, Stripe / Shopify / Amazon settlements, returns / chargeback log, exemption-certificate library | "Where do we have sales-tax nexus?" "How do you reconcile Stripe net deposits to gross revenue?" "Do you handle our Amazon FBA tax exposure?" |
| **Manufacturing** | Standard-cost vs. actual-cost variance schedule, §263A UNICAP workpaper, perpetual-inventory-to-physical-count tie, BOM, R&D project inventory + Form 6765 history | "Do you compute UNICAP each year?" "Do we qualify for §41 R&D?" "Do you support IC-DISC / FDII?" |
| **Healthcare** (medical / dental / vet / optometry) | Net-collection ratio + days-in-AR report, payer mix, contractual-adjustment log, HIPAA business-associate agreement (BAA), DEA registration, CMS provider numbers, equipment §179 schedule | "Are you HIPAA-aligned for our PHI?" "Do you sign a BAA?" "Can you handle our cost report?" |
| **Nonprofit** (501(c)(3)) | Form 1023 / 1024 + determination letter, board policies (conflict, whistleblower, document retention), donor-restriction documentation, grant agreements, functional-expense allocation method, prior 990s | "Will you handle our Schedule A public-support test?" "Do you handle UBIT / 990-T?" "How do you allocate functional expenses?" |
| **Real estate** | Cost-segregation studies, 1031 exchange documentation, passive-activity election history, §469 / §163(j) elections, depreciation-recapture schedule, cap-rate / NOI workpapers | "Should we do a cost-seg study this year?" "How do you handle 1031 timing?" "Are we a real-estate professional under §469?" |
| **Financial services** (RIA / broker-dealer / fund) | Form ADV last filing, surprise custody exam SOC, GIPS performance composites, partnership allocations + §704(c), §1061 three-year holding-period schedule, Form PF if applicable | "Will you handle our surprise custody exam?" "How do you handle §1061?" "Do you handle our SOC 1 reliance review?" |
| **Agriculture** | Schedule F, livestock and crop inventory, §175 soil and water election, March 1 farmer-rule timing, weather-related-sale deferral records, equipment §179 schedule | "Do we qualify for the March 1 farmer rule?" "Should we elect §175?" "Can we average income under §1301?" |
| **Generic small business** | Standard PBC list above; no vertical overlay | Standard FAQ |

**Process:**

1. **Confirm acceptance and continuance.** Run the conflict-check screen (against the firm's existing client list and any related parties), the AML / sanctions screen (OFAC SDN list at minimum), and document the firm's acceptance decision. For first-year attest engagements, record the AU-C 210 predecessor-auditor communication step, the date of the request, the predecessor's response, and access-to-prior-workpapers status. Flag any item in the engagement that may create an independence conflict (AICPA Code §1.295 — non-attest services to attest clients).
2. **Welcome letter.** Warm, professional introduction confirming services engaged (using the firm's `voice` from `config.yml`), introducing the team (firm_partner, engagement_lead, client_success_lead, default_engagement_team — all from config), setting expectations for communication cadence (response-time SLA from config), and naming the primary portal URL (from config). One-paragraph version for sole prop / SMLLC clients; two-paragraph version for entities with a finance team.
3. **Document request checklist.** Build the checklist by reading the matrix above for the engagement's service column × the client's entity row. Layer the industry-overlay items on top. Then layer any conditional items triggered by the input. Group the final list into (a) **Day 1 — required to start**, (b) **Within 30 days**, and (c) **Reference / one-time**. For each item, name the specific format expected (PDF, CSV, native QBO file, etc.) and the intake method (portal upload, encrypted email).
4. **30 / 60 / 90-day service timeline.** Build a milestone framework specific to the service:
   - **Tax prep**: D1 portal access, D7 organizer received, D14 first review pass, D30 questions returned, D45 draft return, D60 e-sign + filing
   - **Bookkeeping / CAS**: D1 software access, D7 historical clean-up scope, D14 chart-of-accounts review, D30 first month-end close, D60 second close + first management package, D90 first quarterly review
   - **Audit (first-year)**: D1 acceptance + AU-C 210, D14 planning meeting + materiality, D30 risk-assessment workpapers, D45 interim fieldwork, D60 year-end fieldwork start, D90 fieldwork complete + draft report
   - **Compilation / Review (SSARS §70 / §90)**: D1 acceptance + independence assessment, D7 PBC list, D14 trial balance + supporting docs received, D30 first-pass review, D45 management discussion, D60 issuance
   - **Payroll**: D1 system access, D3 prior-payroll history loaded, D7 first parallel run, D14 first live run, D30 first 941 / state UC reconciliation
   - **Advisory**: D1 scope confirmation, D14 baseline data gather, D30 first deliverable, D60 second deliverable + recurring cadence agreed
   Express milestones as specific dates pegged to the engagement start date, not generic week numbers.
5. **Key contacts sheet.** A single page with: firm_partner (signing partner), engagement_lead (primary contact), client_success_lead (administrative contact), billing contact, after-hours / emergency contact (if applicable to engagement type), and the firm's standard response-time SLA. Each row has name, title, direct phone, email, and what to bring to that person.
6. **Technology setup playbook.** Use the per-software row above. Walk through each step the client must perform to grant the firm access, connect bank feeds, configure document sharing, and set up the closed-period lock. Include MFA-enforcement check, API integration map, and FTC Safeguards alignment notes. For each system that writes to the GL, list the read-access confirmation step.
7. **FAQ.** Five general FAQs (response-time, what to do if I get a notice, how do I send sensitive documents, when will I get my deliverables, how do I escalate) plus the vertical-specific FAQs from the industry overlay table. Each FAQ is one to three sentences — not a wall of text.
8. **Firm-side risk-management items (engagement file).** A separate internal section the firm retains: conflict-check results, AML / sanctions screen results, AU-C 210 record (first-year attest), independence assessment (AICPA Code §1.295), §6695 conflict-of-interest waiver if applicable, FTC Safeguards Rule alignment confirmation, §7216 consent capture (if cross-sell or third-party disclosure anticipated), and prior-firm-deliverables reconstruction notes (when transitioning).
9. **Prior-period reconstruction (transition clients only).** When the client is transitioning from another firm or from self-prepared books, generate a reconstruction worksheet listing what the firm needs to verify before relying on prior numbers: opening balances, fixed-asset basis, intangibles amortization schedule, retained earnings rollforward, prior-period adjustments, and any unfiled returns or open IRS / state notices.
10. **Cross-skill handoffs (partner-facing addendum, separate from the client-facing package unless the engagement authorizes client communication).** After the risk-management appendix, list every downstream skill that should be invoked — with the specific trigger:
    - **Engagement Letter Generator** — trigger if the engagement letter is not yet signed; route entity type, service mix, and fee structure so the letter's entity-type × service-type document matrix and AI-Tool Disclosure clause align with this package.
    - **Compliance Tracker** — trigger always on a new engagement; route the entity type, state(s), service mix, and fiscal year-end so the firm's multi-jurisdiction compliance calendar is seeded on day one (PTET, annual report, franchise tax, sales-tax, payroll deposits, 990 / Schedule A).
    - **Sales Tax Nexus Analyzer** — trigger when the client has multistate revenue, marketplace-facilitator sales, or e-commerce / SaaS sales-by-state exposure surfaced in the conditional PBC items; route the sales-by-state report and transaction mix.
    - **Client Email Drafter** — trigger for the first client-facing communication after signing (welcome cadence, first document-request nudge, portal-access walkthrough); route the SLA, portal URL, and document-request list.
    - **Transaction Categorizer** — trigger for any bookkeeping / CAS engagement; route the ledger backbone (QBO / IAS / IES / IES Construction Edition / Xero / Sage Intacct / NetSuite per the playbook row that fired), chart-of-accounts template, and capitalization threshold so categorization rules are seeded to the chosen backbone.
    - **Month-End Checklist** — trigger at the first close for bookkeeping / CAS clients; route the entity type and software so the close checklist's entity-type overlay is correct.
    - **R&D Credit Documenter** — trigger when the R&D-credit conditional PBC items fire (§41 / §174A); route the project-inventory and time-tracking-system setup requests.
    - **Audit Planning Memo + Fraud Risk Brainstorm + Going Concern Assessment** — trigger together for first-year attest engagements once AU-C 210 predecessor communication is logged; route the acceptance file and any risk-of-material-misstatement concerns surfaced during acceptance.
    - **Cash Flow Forecaster** — trigger when the engagement is value-priced / retainer or when the client flags liquidity concern during intake; route the fee schedule and any known obligations.
    - Route the segment (from `client_segment_routing`) and the Intuit tier (from `firm_intuit_partner_tier`) alongside each handoff so downstream skills inherit the same backbone and segment assumptions.

**Output requirements:**

- Each section clearly separated with headings; client-facing sections (welcome letter, document request, timeline, contacts, tech setup, FAQ) come first; the firm-side risk-management section is a separate appendix the partner retains
- Professional tone that is welcoming but organized; voice matches `config.yml` → `voice`
- All deadlines expressed as specific dates pegged to the engagement start date (not "Q1" or "Week 4"); use the firm's calendar conventions from config
- **Entity-type-specific carve-outs** — payroll-tax deposits and W-2 / W-3 timing for employers; K-1 distribution timing for partnerships and S-corps; 1041 / K-1 timing for trusts; 990 / Schedule A timing for nonprofits; reasonable-comp documentation expectation for S-corp owner-employees; SMLLC disregarded-entity reporting; Schedule F treatment for sole-prop farmers
- **Service-type-specific deliverables** — name the deliverable artifact (return, financial statements, monthly package, audit report, advisory memo) and the issuance date for each
- **Software-specific access steps** — use the per-software row above; include the MFA enforcement check
- **Industry-specific overlay** — pull the row matching the client's vertical from the overlay table
- **Prior-firm transition handling** — when the client is transitioning, include AU-C 210 + prior-period reconstruction explicitly
- **Risk-management appendix** — conflict check, AML screen, independence, FTC Safeguards, §7216 — partner-signable
- **Software-specific access steps for the Intuit tier in use** — when the client (or the firm's provisioning) is on IAS, IES, or IES Construction Edition, use that playbook row rather than the QBO / QBD rows; reflect `firm_intuit_partner_tier` provisioning rights and `firm_qboa_to_ias_migration_status`
- **Cross-skill handoff addendum** — partner-facing, separate from the client-facing package unless the engagement authorizes client communication; lists each triggered downstream skill with its trigger and the segment / Intuit-tier context to inherit
- Save the package to `outputs/onboarding/{ClientSlug}-{StartDate}/` with a top-level cover sheet, the client-facing package as a single document, the risk-management appendix as a separate file the partner countersigns, and the cross-skill handoff addendum as a partner-facing file

## Example Output

> [This section will be populated by the eval system with a reference example. For now, run the skill against a transitioning S-corp restaurant client moving from spreadsheets to QBO + Gusto, with prior-firm engagement letters on file but no prior reviewed financials, and verify that (a) the document checklist pulls Tax Prep + Bookkeeping/CAS + Payroll columns AND the Restaurant overlay (8027, tip log, POS export), (b) the QBO row drives the tech-setup section, (c) the AU-C 210 step appears because the client is transitioning, (d) the timeline includes both the bookkeeping cleanup (D7) and the first-month close (D30), and (e) the risk-management appendix lists conflict check, AML / OFAC screen, and FTC Safeguards confirmation as separate sign-off lines.]
