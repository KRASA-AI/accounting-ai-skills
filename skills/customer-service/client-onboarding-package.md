---
name: "Client Onboarding Package"
category: customer-service
tools: [claude, chatgpt]
difficulty: beginner
time_saved: "~25 min/client"
version: 1.0
last_eval_score: null
---

# 📋 Client Onboarding Package

## Purpose

Generate a complete new-client onboarding package including a welcome letter, document request checklist, service timeline, key contact sheet, and technology setup instructions — all personalized to the client's entity type and service scope.

## When to Use

Use this skill when signing a new client to any service — tax preparation, bookkeeping, CAS, audit, payroll, or advisory. Also helpful when transitioning a client from another firm and you need to systematize the information-gathering process.

## Required Input

Provide the following:

1. **Client details** — Business name, entity type (sole prop, LLC, S-corp, C-corp, nonprofit, partnership), industry, and primary contact name
2. **Services engaged** — Which services you are providing (tax prep, monthly bookkeeping, payroll, advisory, audit, etc.)
3. **Fiscal year-end** — The client's fiscal year-end date
4. **Accounting software** — What the client currently uses (QuickBooks Online, Xero, Sage, spreadsheets, etc.) or what you will set up
5. **Key deadlines** — Upcoming filing or reporting deadlines relevant to the engagement
6. **Any special circumstances** — Prior-year issues, IRS notices, entity changes, first-year business, transition from another firm

## Instructions

You are a skilled accounting professional's AI assistant. Your job is to create a polished, thorough onboarding package that makes new clients feel organized and confident from day one.

**Before you start:**
- Load `config.yml` from the repo root for company details, rates, and preferences
- Reference `knowledge-base/terminology/` for correct industry terms
- Use the company's communication tone from `config.yml` → `voice`

**Process:**

1. **Welcome letter** — Warm, professional introduction confirming services, introducing the team, and setting expectations for communication cadence and response times
2. **Document request checklist** — Tailored to entity type and services. For tax clients: prior returns, W-2s/1099s, K-1s, estimated payments, asset purchase records. For bookkeeping clients: bank access, credit card statements, chart of accounts, prior financials. For audit: prior audit reports, significant contracts, board minutes
3. **Service timeline** — Month-by-month or quarterly view showing what the firm will deliver and when, plus what the client needs to provide by each milestone
4. **Key contacts sheet** — Who on your team handles what, with email and phone, plus expected response times
5. **Technology setup guide** — Step-by-step instructions for granting software access, setting up a client portal, and connecting bank feeds or document sharing (tailored to the specific tools from input)
6. **FAQ section** — Answers to the five most common new-client questions for the relevant service type

**Output requirements:**
- Each section clearly separated with headings
- Professional tone that is welcoming but organized
- All deadlines expressed as specific dates where possible (not just "Q1")
- Entity-specific requirements (e.g., payroll tax deposits for employers, K-1 distribution timelines for partnerships)
- Ready to send with minimal editing
- Saved to `outputs/` if the user confirms

## Example Output

> [This section will be populated by the eval system with a reference example. For now, run the skill with sample input to see output quality.]
