# 03 — Dashboards

## Overview

8+ Power BI dashboards were designed, built, and deployed to production — each with a specific audience, purpose, and data refresh cadence. Every dashboard followed a design standard: navigation index page, consistent filter bar, drill-through pages, and a linked User Guide section.

> ⚠️ Screenshots are not included in this repository to protect the confidentiality of the organization's operational data. Dashboard descriptions below are based on structure, KPIs, and design — not real values.

---

## Dashboard Inventory

| # | Dashboard | Primary Audience | Pages | Refresh | Source |
|---|-----------|-----------------|-------|---------|--------|
| 1 | Sales Report | CEO, Commercial Director, Clinic Managers | 8 | Hourly | Odoo ERP |
| 2 | Budget Execution | CEO, Finance, Operations Director | 4 | Monthly | Odoo + Google Sheets |
| 3 | KPIs Movet | CEO, All Directors | 5 | Daily/Monthly | Multi-source |
| 4 | NPS & PQRS | Marketing, Quality, Customer Service | 3 | Continuous | Google Sheets |
| 5 | Commercial Control Tower | Commercial Director, Marketing | 3 | Monthly | RFM Pipeline |
| 6 | Customer Analysis (RFM) | Marketing, Commercial Director | 2 | Monthly | RFM Pipeline |
| 7 | Inventory & Logistics Control Tower | Operations, Inventory Manager | 2 | Daily | Odoo + Google Sheets |
| 8 | Emergency Line (Beta) | Quality Director, Medical Director | 2 | Real-time | Google Sheets |

---

## Dashboard Detail

### 1 — Sales Report
**The flagship dashboard. 8 pages, designed for daily commercial tracking.**

| Page | Content |
|------|---------|
| Index | Navigation page with section links and data freshness indicator |
| General Tracking | MTD sales vs. budget vs. expected · Gauge chart · Daily trend line · Performance table by clinic |
| Detailed Tracking | Clinic-by-clinic breakdown · Daily sales chart per location vs. daily target |
| Business Unit Tracking | Revenue share by business unit · MoM variation · Unique users by unit |
| Growth Detail | Location-level cumulative revenue (full history since opening) · MoM growth % comparison |
| Products & Services Pt.1 | Top products by revenue · Category breakdown |
| Products & Services Pt.2 | Extended product analysis · Profundización (cross-sell depth) |
| Program & Payment | Sales by loyalty program · Channel · Payment method |

**Key KPIs:** Monthly Goal · Expected Sales · Accumulated Sales · Goal Attainment % · Gap to Goal · Unique Users · Orders · Products Sold · Avg Ticket per User · Basket Size

**Design feature:** Navigation via paw-print icon buttons (on-brand for veterinary context) linking to each section. Home button on every page for quick return to index.

---

### 2 — Budget Execution
**Financial reconciliation dashboard. Built with weekly Finance team validation.**

| Page | Content |
|------|---------|
| Sales | Actual vs. budget vs. expected · Variance · Clinic-level breakdown |
| Users & Ticket | User count vs. target · Avg ticket vs. target · Clinic comparison |
| Ticket | Detailed ticket analysis by business unit and period |
| Trends | Month-over-month trend comparison |

**Key KPIs:** Budget Amount · Actual Sales · Expected Sales · Budget Attainment % · Variance (COP) · Variance %

**Governance note:** This dashboard was the primary tool for the weekly Finance-BI reconciliation process. Any discrepancy between Power BI figures and Finance's manual records triggered a root-cause investigation before publishing updates. This process was what established organizational trust in the data.

---

### 3 — KPIs Movet
**Multi-dimensional executive dashboard across 4 organizational pillars.**

| Page | Content |
|------|---------|
| Sales Distribution | Revenue concentration by clinic · Business unit share · Geographic distribution |
| Commercial KPIs — Dashboard view | Real-time commercial performance indicators |
| Commercial KPIs — Summary view | Condensed scorecard format for quick executive review |
| People KPIs | HR metrics: headcount, turnover, absenteeism, team performance |
| Medical Direction KPIs | Clinical quality indicators, medical procedures, diagnostic categories (CIE-10) |
| Infrastructure KPIs | Facility and equipment availability metrics |

**Audiences by page:** CEO and all directors use pages 1–3. HR uses page 4. Medical Direction uses page 5. Operations uses page 6.

---

### 4 — NPS & PQRS (Customer Satisfaction)
**Voice of the customer — post-service satisfaction and complaint tracking.**

| Page | Content |
|------|---------|
| NPS Report I | Net Promoter Score trend · Promoter/Passive/Detractor breakdown · By clinic |
| NPS Report II | Response volume · Score distribution · Period comparison |
| PQRS Report | Complaint/request tracking · Resolution time · Category breakdown |

**Key KPIs:** NPS Score · Promoter % · Detractor % · Response Rate · Avg Resolution Time

---

### 5 — Commercial Control Tower (RFM)
**Strategic segmentation dashboard. Updated monthly after RFM pipeline runs.**

| Page | Content |
|------|---------|
| Segmentation Overview | RFM segment distribution · Segment size and revenue share |
| Segment Detail | Client counts per segment · Period-over-period movement between segments |
| Strategy View | Recommended actions per segment · Campaign tracking |

**RFM Segments:** Champions · Loyal Customers · Potential Loyalists · At Risk · Can't Lose · Lost

---

### 6 — Customer Analysis
**Client behavior and retention analysis.**

| Page | Content |
|------|---------|
| Recurrence Analysis | Visit frequency distribution · Recency histogram · Patient retention curve |
| CRM Integration | Contact data completeness · Reachability index · Patient profile summary |

---

### 7 — Inventory & Logistics Control Tower
**Stock management and ABC product classification.**

| Page | Content |
|------|---------|
| Inventory Overview | Stock levels by location · ABC classification (A/B/C) · Rotation rate |
| Logistics Detail | Reorder point tracking · Slow-moving stock · Top 20 products by movement |

**ABC Classification:** Products categorized A (high value/rotation), B (medium), C (low) — guides purchasing prioritization and storage allocation.

---

### 8 — Emergency Line (Beta)
**24/7 emergency call monitoring. In development at project close (Phase 1 of 3 completed).**

| Page | Content |
|------|---------|
| Call Monitoring | Real-time call log · Response time · Case type classification |
| Quality Tracking | SLA compliance · Resolution rate · Escalation cases |

**Status at handoff:** 33% complete. Phase 2 (automated alerting) and Phase 3 (trend analysis) remained in backlog.

---

## Design Standards Applied Across All Dashboards

| Standard | Implementation |
|----------|---------------|
| Navigation | Index page on every dashboard · Home button (top right) · Sequential arrows (bottom) |
| Filters | Consistent filter bar: Date range · Clinic (Sede) · Business unit · Period |
| Data freshness | Last refresh timestamp on every page header |
| Drill-through | Right-click drill-through to detail pages where applicable |
| Conditional formatting | Traffic-light coloring on KPI cards (green/yellow/red) vs. target |
| Audience documentation | Every dashboard documented in User Guide with use-case examples |
