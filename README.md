# Accounting AI Skills

**Free, open-source AI prompts and workflows built for accountants.** Clone this repo, point your AI assistant at it, and start saving hours every week.

> Built and maintained by [KRASA AI](https://krasa.ai) — free AI tutorials and skills for every industry.
> See all industries at [krasa.ai/industries](https://krasa.ai/industries).

---

## What's Inside

This repo is a complete AI toolkit for accounting. Every skill is a standalone prompt file that works with **Claude, ChatGPT, or any major AI assistant** — no coding required.

| Skill | What it does | Time saved |
|-------|-------------|------------|
| 🏷️ Transaction Categorizer | Classify and describe uncategorized bank transactions with suggested GL codes. | ~20 min/batch |
| ✉️ Client Email Drafter | Write professional emails requesting missing documents, explaining deadlines, or summarizing work. | ~10 min/email |
| 📄 Engagement Letter Generator | Draft an engagement letter from scope details with standard terms and fee structure. | ~15 min/letter |
| 📝 Tax Memo Writer | Summarize a tax research question with applicable code sections and recommendations. | ~30 min/memo |
| ✅ Month-End Checklist | Generate a client-specific month-end close checklist based on their entity type and accounts. | ~10 min/client |
| 📊 Financial Narrative Builder | Turn raw financials into a management-ready narrative with variance explanations. | ~25 min/report |
| 📊 Cash Flow Forecaster | Build a rolling 13-week cash flow projection with liquidity alerts and collection risk flags. | ~30 min/forecast |
| 📋 Client Onboarding Package | Generate a complete welcome package with document checklists, timelines, and tech setup guides. | ~25 min/client |
| ✅ Compliance Tracker | Create a full regulatory compliance calendar with deadlines, warnings, and responsible parties. | ~20 min/review |

**Total time saved per use: ~185+ minutes across all skills.**

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
