# 02 — Data Architecture

## Overview

With the infrastructure in place, the next challenge was designing a data model that could reliably integrate 10+ sources with different formats, owners, and update frequencies — and serve 8+ dashboards simultaneously without performance degradation.

---

## Data Sources Integrated

| # | Source | Type | Update Frequency | Dashboards Served |
|---|--------|------|-----------------|-------------------|
| 1 | Odoo ERP — Service Orders | ODBC (CData) | Hourly | Sales, Budget, KPIs, RFM, NPS, Commercial |
| 2 | Odoo ERP — Inventory | ODBC (CData) | Daily | Inventory Control Tower |
| 3 | Google Sheets — Budget & Goals | OAuth | Monthly | Budget Execution |
| 4 | Google Sheets — HR KPIs | OAuth | Monthly | KPIs Movet (People) |
| 5 | Google Sheets — NPS Survey Responses | OAuth | Continuous | NPS Dashboard |
| 6 | Google Sheets — Customer Service Cases | OAuth | Continuous | NPS / PQRS |
| 7 | Google Sheets — ABC Inventory Ranking | OAuth | Monthly | Inventory Control Tower |
| 8 | Google Sheets — CRM Data | OAuth | Weekly | Customer Analysis |
| 9 | Google Sheets — RFM Segments | OAuth | Monthly | Commercial Control Tower |
| 10 | Google Sheets — Emergency Line Log | OAuth | Real-time | Emergency Line (beta) |

---

## Centralized Dataflow Architecture

Instead of connecting each dashboard directly to its sources (which would multiply API calls, slow performance, and create maintenance nightmares), all transformation logic was centralized in a **dedicated Dataflow Workspace**.

```
                    DATAFLOW WORKSPACE
                    ─────────────────
  Odoo ERP ──────► movet__data_service_orders  ──────► 8 dashboards
  Google Sheets ─► base_segmentation            ──────► Commercial CT
  Google Sheets ─► base_RFM                     ──────► Customer Analysis
  Odoo ERP ──────► stock_inventory_snapshot     ──────► Inventory CT
  Google Sheets ─► budget_execution_data        ──────► Budget Execution
  Google Sheets ─► nps_survey_data              ──────► NPS Dashboard
  Google Sheets ─► hr_kpi_data                  ──────► KPIs Movet
  Google Sheets ─► emergency_line_data           ──────► Emergency Line
```

**Principle:** Each dataflow is processed once, stored in Power BI's internal storage, and consumed by multiple dashboards. This reduces load on source systems and ensures all dashboards display consistent data.

---

## Core Dataflow — Service Orders (Power Query M)

The most complex and critical dataflow extracts the main service order table from Odoo and applies a full transformation pipeline before dashboards consume it.

**Transformations applied:**

```m
// 1. Connect via ODBC (CData DSN)
Origen = Odbc.DataSource("dsn=CData PBI Odoo", [HierarchicalNavigation = true]),

// 2. Standardize clinic names
// Ensures consistency between Odoo values and dashboard labels
#"Replace PARKWAY" = Table.ReplaceValue(..., "PARKWAY", "PARK WAY", ...),

// 3. Timezone adjustment
// Odoo stores UTC timestamps — Colombian timezone is UTC-5
DateAdjusted = [date] + #duration(0, 5, 0, 0),
DateAdjustedPK = DateTime.Date([DateAdjusted]),   // Pure date for relationships

// 4. Text normalization
// Ensure consistent casing across business unit and product fields
line_unit_business = Text.Upper([line_unit_business]),
line_product_name  = Text.Upper([line_product_name]),

// 5. Business unit homologation
// Merge near-identical categories caused by Odoo naming evolution
// Example: "INMUNOCROMATOGRAFIA" → "LABORATORIO"

// 6. Service type classification
// Binary split: "Servicios médicos" vs "Servicios otros"
// Used for high-level filtering across all dashboards

// 7. Product name standardization
// Normalize long/variable product names into clean reporting labels
// E.g., "HOSPITALIZACIÓN GENERAL (24H)" → "Hospitalización General"

// 8. Category macro-grouping
// Recategorize fine-grained product categories into reportable macro-groups
// E.g., "ANESTESIA ESPECIALIZADA" → "ANESTESIA"

// 9. Error handling
// Replace transformation errors with null to prevent dataflow failures
line_unit_business = Table.ReplaceErrorValues(...)
```

**Refresh schedule:** Hourly from 00:30 AM. Average duration: 8–15 minutes. All dependent dashboards are staggered to refresh after this window closes.

---

## Star Schema Design

The data model follows a **star schema** — a fact table at the center connected to dimension tables — for performance, clarity, and DAX simplicity.

```
                    ┌──────────────┐
                    │  dim_date    │
                    │  (calendar)  │
                    └──────┬───────┘
                           │
┌──────────────┐    ┌──────┴────────┐    ┌──────────────┐
│  dim_clinic  │────│ fact_service  │────│ dim_product  │
│  (locations) │    │   _orders     │    │  (services)  │
└──────────────┘    └──────┬────────┘    └──────────────┘
                           │
                    ┌──────┴───────┐
                    │ dim_business │
                    │    _unit     │
                    └─────────────-┘
```

**Why star schema over flat table?**
- DAX measures calculate correctly against a proper dimensional model
- Relationships enforce referential integrity — no double-counting
- New dimensions (new clinic, new business unit, new product) require only table additions
- Performance: Power BI's Vertipaq engine is optimized for star schema

---

## Monthly Segmentation Pipeline — RFM

The RFM (Recency, Frequency, Monetary) segmentation model required a specific multi-step dataflow execution order to produce correct results.

**Monthly execution sequence (must run in this order):**

```
Step 1: base_segmentation      ← Source data refresh
    ↓
Step 2: clientes_no_contactar  ← Exclusion list applied
    ↓
Step 3: base_RFM               ← Final RFM scores calculated
    ↓
Dashboard: Customer Analysis / Commercial Control Tower refresh
```

**Why strict ordering?** Each step depends on the previous one's output. Running them out of sequence produces stale or incorrect segment assignments. This was formalized as a documented operational procedure (see section 06 — Data Governance).

---

## Google Sheets Integration — Governance Rules

Google Sheets connections require OAuth authentication and are more fragile than ODBC connections. The following rules were established and documented for all Sheets owners:

| Rule | Reason |
|------|--------|
| Never add, delete, or rename columns without coordinating with BI first | Column structure changes break Power Query transformations silently |
| Maintain consistent data types per column | Mixed types (text + numbers in same column) cause transformation errors |
| No blank cells in key columns | Blanks propagate as nulls and distort aggregations |
| Update data on the agreed schedule | Stale Sheets data makes dashboards show outdated figures |

These rules were included in the User Guide distributed to all data owners (see section 05 — Data Literacy).

---

## Python — Extended Analytics

For two specific use cases, Power Query alone was insufficient and Python was used:

**1. RFM Segmentation model**
- Libraries: Pandas, NumPy, Scikit-learn
- Calculated R, F, M scores per customer
- Assigned segments (Champions, Loyal, At Risk, Lost, etc.)
- Output written to Google Sheets consumed by Commercial Control Tower

**2. Client Density Map**
- Libraries: Pandas, Google Maps API, Matplotlib
- Geocoded patient addresses against clinic locations
- Generated heat map visualization of patient density vs. clinic coverage
- Used to support strategic decisions about new clinic location selection
- Output embedded in Power BI as a custom visual layer
