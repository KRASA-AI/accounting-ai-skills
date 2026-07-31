# AI-Generated Workpaper & Spreadsheet Standards

*Added 2026-07-28 by the Landscape Monitor. Concepts extracted and rewritten from a practitioner account of building agentic accounting skills; no source text reproduced verbatim (see `monitor-log/2026-07-28.md`).*

## Why this exists

Several skills in this library produce spreadsheet or schedule-style deliverables (`cash-flow-forecaster`'s CSV companion, `variance-analyzer`'s decomposition tables, and the backlog `Fixed Assets/Depreciation Calculator`). `industry-overview.md` already flags "formula-based outputs, not hardcoded values" as a 2026 governance trend. This file makes that principle operational: four concrete conventions a skill should follow whenever the AI is generating a number a client, reviewer, or auditor will rely on.

These conventions exist because AI-drafted financial output fails in predictable ways — silent misclassification, inconsistent formula-vs-hardcode behavior, and no verification step before delivery. Each convention below closes one of those failure modes. They complement, rather than replace, AU-C 230 / AS 1215 workpaper-documentation expectations: the goal is that an AI-drafted workpaper should require the same reviewer trust as one built by a first-year staff accountant with a documented, followable process.

## 1. No silent classification — required fields over inferred defaults

When a skill's output depends on a judgment call that changes which accounts or treatment apply (AFS vs. trading securities, capital vs. repair, W-2 wages vs. distributions for an S-corp owner), that judgment call must be a **required input**, not something the AI infers from context and moves on. An inferred guess that's wrong doesn't surface as an error — it surfaces as a journal entry hitting the wrong account, discovered (if ever) at review or audit.

**Applies to:** `transaction-categorizer` (entity-aware wage/draw/distribution splits — already partially implemented), the backlog Fixed Assets/Depreciation Calculator (depreciation method, useful life convention, salvage-value treatment), any future investments or lease-classification skill.

**Pattern:** list the judgment-call fields explicitly in the skill's "Required Input" section with "no default — must be confirmed" language, rather than folding them into general context.

## 2. Formula integrity as a pre-delivery gate, not an assumption

A spreadsheet deliverable that contains hardcoded numbers where a reviewer expects formulas defeats the purpose of a spreadsheet — nothing recalculates, and errors compound silently. Any skill that produces a CSV/XLSX with derived figures should treat "does every derived cell resolve as a live formula, with no `#REF!`/`#DIV/0!`/circular-reference errors" as a **checked gate before the file is called done**, not a hoped-for property of the generation step.

**Applies to:** `cash-flow-forecaster`'s optional CSV companion, `variance-analyzer`, the backlog depreciation/fixed-assets skill.

**Pattern:** when a skill's environment supports code execution, add a validation step — recalculate the workbook (e.g., headless spreadsheet recalculation) and confirm no error cells before presenting the file — and say so explicitly in the skill's Instructions section, not just the Output description.

## 3. Input/formula visual separation

Auditors and reviewers need to see, at a glance, what was typed in versus what was calculated. A simple convention — one consistent font color or cell style for raw inputs, a different one for formula cells — makes a workpaper self-documenting without extra narrative.

**Applies to:** any skill producing XLSX output where a human will need to trace a number back to its source (variance analysis, cash flow forecasting, depreciation schedules).

## 4. Single source of truth across related skills

Where two or more skills in this library would each need the same firm- or client-specific reference data — a chart of accounts, entity list, or fee schedule — that data should live in one place the skills share (this repo's `config.yml` / client-specific setup layer), not be re-specified independently inside each skill. Two skills quietly drifting out of sync on the "same" chart of accounts is a classic source of miscategorized output that's hard to catch because each skill looks internally consistent.

**Applies to:** `transaction-categorizer` and any future skill that also needs the client's chart of accounts (e.g., the backlog Fixed Assets/Depreciation Calculator, a future bank-rec helper) — confirm both draw from the same client setup rather than asking the user to re-supply the account list per skill.

## Adoption note

These are documentation standards for skill authors, not a mandate to rewrite existing skills immediately. Apply them when a covered skill is next touched (v-next revision) or when building a new skill that produces spreadsheet/schedule output. Retrofitting all four into every existing skill in one pass was assessed as lower value than applying them going forward — flagged here so the Skill Evaluator can check for them in future scoring passes.
