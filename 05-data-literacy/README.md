# 05 — Data Literacy & Adoption

## Overview

The most technically perfect BI system fails if people don't use it. Data literacy and user adoption were treated as first-class deliverables — not optional extras after the dashboards were built. This section documents the programs, materials, and philosophy behind turning 15+ executives into autonomous data users.

---

## The Adoption Challenge

When the dashboards launched, the organization faced a common problem: **people trusted Excel because they understood it**. Power BI was new, the numbers sometimes looked different from what they were used to, and there was natural resistance to changing a familiar workflow.

Three things broke that resistance:

1. **Financial reconciliation** — proving the numbers were right before asking anyone to trust them
2. **The User Guide** — removing the "I don't know how to use it" barrier
3. **The Data Clinic** — hands-on sessions that turned users from observers into practitioners

---

## The Data Clinic Program

**"Clínica de Datos"** — a recurring training program designed to build data literacy across all organizational levels.

**Format:**
- Live sessions (recorded for async viewing)
- 60–90 minutes per session
- Hands-on: participants navigated dashboards in real time, not just watching a demo
- Q&A built into each session
- Sessions recorded and stored for onboarding new executives

**Audience tiers:**

| Tier | Audience | Content focus |
|------|----------|---------------|
| Executive | CEO, Directors | Strategic KPIs · How to read trends · Decision frameworks |
| Operational | Clinic Managers, Area Leads | Daily/weekly tracking · Goal monitoring · Drill-through to detail |
| Technical | Finance, IT | Data sources · Refresh schedules · Data quality rules |

**Topics covered across sessions:**
- What is Power BI and how to access the workspace
- How data flows from Odoo to dashboards (non-technical explanation)
- Navigation: index pages, filters, drill-through, home button
- How to interpret each KPI (definition, formula logic, what it means for decisions)
- Common misinterpretations and how to avoid them
- How to request new reports (ANS process)

**Outcome:** By month 6, executives were independently using dashboards to prepare for board meetings, weekly reviews, and operational decisions — without requiring BI team support for basic queries.

---

## User Guide — 94-Page Reference Document

A comprehensive written guide covering every dashboard, every page, and every KPI — designed for both onboarding and ongoing reference.

**Structure:**

| Section | Content |
|---------|---------|
| Introduction | Purpose · How to access Power BI workspace · How to use the guide |
| Power BI Basics | Navigation types · Filter usage · Drill-through · Cross-filtering |
| Data Sources | What data exists · Where it comes from · Update frequencies |
| Glossary | 20+ business terms defined for the organization's context |
| Dashboard Catalog | Detailed documentation per dashboard (see below) |
| Contact & Support | How to request help · ANS process · Escalation path |

**Per-dashboard documentation format:**

Each of the 4 primary dashboards received its own section containing:
- Dashboard overview and purpose
- Target audience
- Page-by-page visual inventory (each chart/table described)
- KPI definitions with plain-language explanations
- Step-by-step use case examples ("How to answer: Which clinic is furthest from its monthly goal?")
- Common mistakes and how to avoid them
- Knowledge quiz (interactive questions to validate understanding)

**Design principle:** The guide was written for a non-technical audience. No Power BI jargon, no technical acronyms — plain language that a clinic manager with no data background could follow independently.

**Knowledge Quizzes:**
Each dashboard section ended with an interactive quiz (embedded in the document) testing whether the reader could apply what they learned. This made the guide a learning tool, not just a reference manual.

---

## Data Access Model

Not all users needed access to all data. A tiered access model was implemented:

| Role | Access Level | Dashboards |
|------|-------------|-----------|
| CEO | Full access | All dashboards · All clinics |
| Directors | Department scope | Relevant dashboards · All clinics |
| Clinic Managers | Location scope | Sales Report · KPIs (filtered to their clinic) |
| Finance | Full financial access | Budget Execution · Sales Report |
| Marketing | Customer data access | NPS · Customer Analysis · Commercial CT |
| IT | Infrastructure monitoring | Gateway status · Dataflow admin |

**Implementation:** Power BI Row-Level Security (RLS) was configured to automatically filter data by user role. A clinic manager logging in sees only their clinic's data — without needing manual filter setup.

---

## Measuring Adoption

| Metric | Target | Achieved |
|--------|--------|---------|
| Executives trained | 10+ | **15+** |
| Dashboards in regular use | 4 | **8+** |
| Weekly active users | — | CEO + all directors daily |
| Support requests (post-training) | — | Significantly reduced after guide launch |
| Dashboard autonomous usage | — | Executives preparing board presentations independently |

---

## Key Philosophy

> *"The goal was never to be the guardian of data. It was to make data so accessible that the organization didn't need me to interpret it for them."*

This philosophy shaped every decision in the literacy program:
- Documentation written to survive the original developer's departure
- Training sessions designed to create independence, not dependency
- User guide maintained as a living document with planned update cycles
- Onboarding process established for every new executive joining the organization

The combination of technical reliability (governance) + accessibility (user guide) + hands-on practice (Data Clinic) produced the culture shift from intuition-based to data-driven decision making.


