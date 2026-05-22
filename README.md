# BI Infrastructure — Built from Scratch
### End-to-End Business Intelligence System for a 10+ Location Veterinary Clinic Network

> ⚠️ **Anonymized Case Study**  
> All data, metrics, client names, and internal references in this repository have been anonymized or replaced with illustrative values. This case study is shared exclusively to demonstrate technical skills, architectural decisions, and methodology. It does not represent or disclose any confidential or proprietary information of the organization where this work was performed.

---

## The Challenge

When I joined this organization in March 2024, they operated **10+ veterinary clinic locations across Colombia** with **zero data infrastructure**. Every report was built manually in Excel, took 2+ days to produce, and contained frequent errors. Executives made strategic decisions based on intuition rather than reliable data.

The mandate: **build a complete, production-ready BI system from scratch** — architecture, data pipelines, dashboards, governance, and user adoption — in under 6 months.

---

## What Was Built

```
From:  100% manual Excel reports · 2-day report cycle · Zero data reliability
  To:  Real-time automated dashboards · 8+ hours/week saved · 15+ executives using data daily
```

| Area | Deliverable |
|------|-------------|
| 🏗️ Infrastructure | Power BI Premium · On-premises Gateway (2 nodes) · CData Odoo Connector |
| 🔄 Data Architecture | 10+ data sources · Centralized Dataflows · Star schema model |
| 📊 Dashboards | 8+ executive dashboards across Sales, Budget, KPIs, NPS, RFM, Inventory |
| 📋 Data Governance | KPI catalog · Data dictionary · SLAs (ANS) · Operational procedures |
| 🎓 Data Literacy | "Data Clinic" program · 94-page User Guide · 15+ executives trained |

---

## Architecture Overview

```
┌─────────────────────────────────────────────────────────────────┐
│                        DATA SOURCES                             │
│                                                                 │
│  Odoo ERP ──────────────────────────────────────────────────┐  │
│  (service orders, inventory, patients, billing)             │  │
│                                                             │  │
│  Google Sheets (x8) ────────────────────────────────────┐  │  │
│  (budgets, KPIs, NPS surveys, RFM, ABC inventory,       │  │  │
│   HR indicators, CRM, emergency line)                    │  │  │
└──────────────────────────────────────────────────────────│──│──┘
                                                           │  │
                    ┌──────────────────────────────────────┘  │
                    │                  ┌──────────────────────┘
                    ▼                  ▼
┌───────────────────────────────────────────────────────────┐
│              ON-PREMISES DATA GATEWAY                     │
│         Node 1: Bogotá (primary) · Node 2: Barranquilla  │
│              Always-on · Standard Enterprise Mode        │
└───────────────────────────────────────────────────────────┘
                           │
                           ▼
┌───────────────────────────────────────────────────────────┐
│           POWER BI PREMIUM — DATAFLOW WORKSPACE          │
│                                                           │
│  movet__data_service_orders  ──── Hourly refresh         │
│  base_segmentation           ──── Monthly refresh        │
│  base_RFM                    ──── Monthly refresh        │
│  stock_inventory_snapshot    ──── Daily refresh          │
│  + 4 additional dataflows                                │
│                                                           │
│  Power Query (M) transformations:                        │
│  • Date adjustments · Text normalization                 │
│  • Business unit classification · Error handling        │
│  • Star schema modeling                                  │
└───────────────────────────────────────────────────────────┘
                           │
                           ▼
┌───────────────────────────────────────────────────────────┐
│           POWER BI PREMIUM — REPORT WORKSPACE            │
│                                                           │
│  📊 Sales Report          📊 Budget Execution            │
│  📊 KPIs Movet            📊 NPS & PQRS                  │
│  📊 Commercial Control Tower  📊 Customer Analysis (RFM) │
│  📊 Inventory Control Tower   📊 Emergency Line (beta)   │
└───────────────────────────────────────────────────────────┘
                           │
                           ▼
┌───────────────────────────────────────────────────────────┐
│                    END USERS                              │
│   CEO · Operations Director · Commercial Director        │
│   Medical Director · Clinic Managers · Finance           │
└───────────────────────────────────────────────────────────┘
```

---

## Impact & Results

| Metric | Before | After |
|--------|--------|-------|
| Report generation time | 2 days (manual) | Real-time automated |
| Manual reporting hours saved | — | **8+ hours/week** |
| Executives using dashboards autonomously | 0 | **15+** |
| Data sources integrated | 0 | **10+** |
| Dashboards in production | 0 | **8+** |
| Budget discrepancies identified | Undetected | Caught via data reconciliation |
| Architecture scalability | 10 locations | **Designed for 27+ locations** |

---

## Repository Structure

```
bi-infrastructure-case-study/
│
├── 01-infrastructure/
│   └── README.md     ← Power BI Premium · Gateway · CData Connector · Design decisions
│
├── 02-data-architecture/
│   └── README.md     ← Dataflows · Star schema · 10 data sources · Power Query (M)
│
├── 03-dashboards/
│   └── README.md     ← 8 dashboards: purpose · pages · KPIs · audience · refresh cadence
│
├── 04-data-governance/
│   └── README.md     ← KPI catalog · Data dictionary · SLAs · Operational procedures
│
└── 05-data-literacy/
    └── README.md     ← Data Clinic program · User Guide · adoption model
```

---

## Tech Stack

| Category | Tools |
|----------|-------|
| BI & Visualization | Power BI Premium (Dataflows, Star Schema, DAX, RLS, Workspaces) |
| ERP Integration | Odoo ERP · CData Power BI Connector for Odoo (ODBC) |
| Gateway | On-premises Standard Gateway (Enterprise Mode) · 2 nodes |
| Data Sources | Google Sheets (OAuth) · Odoo ODBC · Excel |
| Transformation | Power Query (M Language) · Python (RFM, density maps) |
| Other | Google Maps API · Python (Pandas, Matplotlib, Scikit-learn) |

---

## Key Lessons Learned

> BI success is not just technical. It requires understanding the business before touching any tool, building trust in data through rigorous validation, documenting exhaustively for scalability, and empowering users — not creating dependency.

1. **Diagnose before building.** Month 1 was entirely stakeholder interviews and data source mapping — no dashboard work.
2. **Validate before publishing.** Weekly Finance reconciliation was the trust-building mechanism that made adoption possible.
3. **Document for your successor.** Every dataflow, every KPI definition, every SLA was written as if someone else would maintain it from day one.
4. **Train, don't gatekeep.** The "Data Clinic" training program was as important as the technical build.

---

## Connect

**Fredys Caballero** · Business Intelligence Analyst · Data Analyst  
[LinkedIn](https://linkedin.com/in/fcaballerosoto) · [GitHub Portfolio](https://github.com/fcaballerodata) · fredyscaballero@gmail.com
