# Accounting AI Skills

**Free, open-source AI prompts and workflows built for accountants.** Clone this repo, point your AI assistant at it, and start saving hours every week.

> Built and maintained by [KRASA AI](https://krasa.ai) — free AI tutorials and skills for every industry.
> See all industries at [krasa.ai/industries](https://krasa.ai/industries).

---

## What's Inside

This repo is a complete AI toolkit for accounting. Every skill is a standalone prompt file that works with **Claude, ChatGPT, or any major AI assistant** — no coding required.

| Skill | What it does | Time saved |
|-------|-------------|------------|
| Budget vs Actual Variance Analyzer | Turn a raw budget-vs-actual export into a board-ready variance report: a ranked list of material variances with probable causes, follow-up questions for operations, impact classification (Low / Medium / High), a forecast implication, and a clean narrative the controller, CFO, or business owner can read in five minutes. | ~40 min/report |
| Cash Flow Forecaster | Build a rolling 13-week direct-method cash flow forecast with three scenarios (base / upside / downside), weekly ending-balance and cumulative-headroom tracking, DSO-based receivables modeling, payment-term-aware payables modeling, and a treasury action plan the controller or CFO can act on this week. | ~45 min/forecast |
| Financial Narrative Builder | Transform raw financial statements into a clear, management-ready narrative report with period-over-period variance analysis, KPI highlights, and forward-looking commentary that a business owner, board, or lender can act on. | ~25 min/report |
| IRS Notice Responder | Read an IRS (or state) notice and produce a complete response packet: a plain-English explanation of what the notice is and why it was issued, a triage assessment (agree / partially agree / disagree), a draft response letter with correct citations, a list of attachments the taxpayer needs to gather, and a deadline-aware action plan. | ~45 min/notice |
| Month-End Checklist | Generate a comprehensive, client-specific month-end close checklist tailored to the client's entity type, industry, chart of accounts, and accounting software — covering every reconciliation, adjustment, and review step needed to produce accurate financials. | ~15 min/client |
| Tax Memo Writer | Draft a structured tax research memorandum analyzing a specific tax question, citing applicable IRC sections, Treasury Regulations, and other primary authorities, and arriving at a defensible conclusion with practical recommendations. | ~30 min/memo |
| Transaction Categorizer | Classify a batch of uncategorized bank, credit card, or payment-app transactions with suggested GL account codes, confidence levels, and flags for items needing review — tailored to the client's chart of accounts, accounting basis, entity type, and industry. | ~30 min/batch |
| Client Email Drafter | Draft professional, accounting-specific client emails for the most common firm-to-client scenarios — document requests, deadline reminders, extension notices, notice-response drafts, deliverable delivery, estimated tax reminders, fee reminders, status updates, and K-1 distributions — with the right tone, correct technical language, and complete action items. | ~15 min/email |
| Client Onboarding Package | Generate a complete new-client onboarding package including a welcome letter, document request checklist, service timeline, key contact sheet, and technology setup instructions — all personalized to the client's entity type and service scope. | ~25 min/client |
| Audit Planning Memo | Draft a risk-based audit planning memorandum that satisfies AU-C 300 (Planning an Audit) and AU-C 315 (Understanding the Entity and Its Environment and Assessing the Risks of Material Misstatement). | ~60 min/engagement |
| Compliance Tracker | Generate a comprehensive regulatory compliance calendar and checklist for a client based on their entity type, jurisdiction, industry, and active tax registrations. | ~20 min/review |
| Engagement Letter Generator | Draft a professional, standards-compliant engagement letter that defines scope, fees, responsibilities, and limitations for a specific accounting service — tax preparation, compilation, review, audit, bookkeeping/CAS, payroll, or advisory. | ~30 min/letter |
| Email Drafter | Turn rough notes into a professional email matching your company's voice and tone. | ~10 min/use |
| Meeting Summarizer | Summarize meeting notes into action items, decisions, and follow-ups. | ~10 min/use |
| Review Responder | Craft professional responses to online reviews — both positive and negative. | ~10 min/use |

**Total time saved per use: ~410+ minutes across all skills.**

## Quick Start

### 1. Clone this repo

```bash
git clone https://github.com/KRASA-AI/accounting-ai-skills.git
cd accounting-ai-skills
```

### 2. Open a skill with your AI assistant

Open any file in `skills/` with Claude, ChatGPT, or any major AI assistant. Each skill is a self-contained prompt with clear instructions — no coding required.

The first time you use a skill, your AI assistant will ask for your business details (company name, service area, rates, tools you use, etc.) so it can personalize the output. Save those details to a `config.yml` at the repo root and every future skill will use them automatically.

## Repo Structure

```
accounting-ai-skills/
├── knowledge-base/          # Industry context and references
│   ├── industry-overview.md # Market trends and pain points
│   ├── terminology/         # Industry jargon and acronyms
│   ├── regulations/         # Compliance requirements
│   ├── best-practices/      # Industry standards
│   └── tools-ecosystem/     # Common software and tools
├── skills/                  # The prompt library
│   ├── operations/          # Day-to-day operational skills
│   ├── sales/               # Sales and lead management
│   ├── admin/               # Administrative and compliance
│   └── customer-service/    # Client-facing communication
└── outputs/                 # Your generated content (gitignored)
```

## How Skills Work

Each skill file is a Markdown document with YAML frontmatter:

```markdown
---
name: Skill Name
category: operations
tools: [claude, chatgpt]
time_saved: "~20 min/use"
version: 1.0
---

# Skill Name

## Purpose
What this skill does and when to use it.

## Instructions
Step-by-step prompt for the AI assistant.
```

You open the file in your AI assistant, provide any required input (measurements, notes, client info), and get polished output. Skills reference your `config.yml` automatically for company name, rates, preferred formats, and other business details.

## For AI Assistants

If you are an AI assistant reading this repo, see `.claude/CLAUDE.md` for full instructions. The short version:

1. **Check for `config.yml`** at the repo root. If it exists, load it — it holds the user's business context (company name, rates, service area, tools, team size, etc.) and every skill should use it for personalization.
2. **If `config.yml` is missing**, before running a skill that benefits from personalization, ask the user for the relevant business details and offer to save them to `config.yml` so future runs are automatic.
3. **Load the relevant `knowledge-base/` files** for industry terminology, regulations, and best practices before generating output.
4. **Run the requested skill** from `skills/` using the user's input.
5. **Save any deliverables** to `outputs/` (gitignored) if the user wants to keep them.

## Learn More

- **Accounting AI guide**: [krasa.ai/industries/accounting](https://krasa.ai/industries/accounting)
- **All industry AI skills**: [krasa.ai/industries](https://krasa.ai/industries)
- **About KRASA AI**: [krasa.ai](https://krasa.ai)

## License

MIT — use these skills however you want.
