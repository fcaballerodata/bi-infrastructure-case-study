# 04 — Data Governance

## Overview

A BI system without governance is just a collection of dashboards. Governance was built as a first-class deliverable — not an afterthought — covering KPI definitions, data quality standards, service agreements, and operational procedures that made the system maintainable and trustworthy beyond the original builder.

---

## 1 — KPI Catalog

A centralized spreadsheet (`Medidas & Documentacion`) served as the single source of truth for every metric in the system.

**Structure per KPI entry:**

| Field | Description |
|-------|-------------|
| KPI Name | Official name as displayed in dashboards |
| Definition | Business definition in plain language |
| DAX Formula | Exact Power BI measure code |
| Data Source | Which dataflow/table the metric reads from |
| Dashboard(s) | Where this KPI appears |
| DRI | Directly Responsible Individual — who owns the number |
| Update Frequency | How often the source data refreshes |
| Validation Rule | How to verify the number is correct |

**Why a KPI catalog matters:** Without it, two different people ask "what was last month's revenue?" and get two different answers — each correct by their own logic, but inconsistent across the organization. The catalog eliminated that ambiguity.

**Example of KPI governance applied:**
> *"Ventas acumuladas"* (Accumulated Sales) is defined as net sales after discounts and before taxes, as extracted from Odoo service orders with status "done" and within the selected date range. It excludes cancelled orders, internal transfers, and test transactions.

Every variation of "sales" that existed in informal Excel reports was mapped to one of three official definitions and documented.

---

## 2 — Data Dictionary

A field-level reference document for the primary tables in the data model.

**Tables documented:**

| Table | Description |
|-------|-------------|
| `service_orders` | Core fact table — all veterinary service transactions from Odoo |
| `base_RFM` | Monthly customer segmentation scores (Recency, Frequency, Monetary) |
| `stock_inventory_snapshot` | Daily inventory state by product and location |
| `budget_execution` | Monthly budget targets by clinic and business unit |

**Structure per field:**

| Column | Type | Description | Example values | Null allowed |
|--------|------|-------------|---------------|--------------|
| `date_adjusted` | datetime | Service date, timezone-corrected to UTC-5 | 2025-01-15 09:30 | No |
| `warehouse_name` | text | Clinic name (standardized) | "CEDRITOS", "CALLE 80" | No |
| `line_unit_business` | text | Business unit (homologated) | "HOSPITAL", "LABORATORIO" | Yes |
| `line_product_name_set` | text | Standardized product name | "Hospitalización General" | Yes |
| `line_category_set` | text | Macro product category | "ANESTESIA", "ORTOPEDIA" | Yes |
| `tipo_servicio` | text | Service classification | "Servicios médicos", "Servicios otros" | No |

---

## 3 — Business Unit Classification

One of the most important governance decisions was standardizing the business unit taxonomy across Odoo, Google Sheets, and dashboards.

**Approved business unit list (homologated):**

| Unit | Category |
|------|----------|
| HOSPITAL | Servicios médicos |
| LABORATORIO | Servicios médicos |
| QUIRÚRGICO | Servicios médicos |
| IMAGENOLOGÍA | Servicios médicos |
| CONSULTA | Servicios médicos |
| FARMACIA | Servicios otros |
| ALIMENTO | Servicios otros |
| GROOMING | Servicios otros |
| SNACKS | Servicios otros |
| ESTÉTICA E HIGIENE | Servicios otros |
| ACCESORIOS | Servicios otros |

**Mapping rules documented:** Any new Odoo category that doesn't match this list triggers a BI review before being added to dashboards.

---

## 4 — SLAs (Acuerdos de Nivel de Servicio / ANS)

Formal service level agreements were established between the BI area and internal stakeholders. This was a significant organizational step — formalizing BI as a service with defined response times, not just an ad-hoc support function.

**Request classification by complexity:**

| Level | Description | Commitment |
|-------|-------------|-----------|
| Low | Simple filter change, label update, color adjustment | 1–2 business days |
| Medium | New page, new KPI, new data connection to existing source | 3–5 business days |
| High | New dashboard, new data source integration, model changes | 2–3 weeks |
| Critical | Production dashboard not loading, data not refreshing | Same day response |

**Request process established:**
1. Requester submits via agreed channel (not WhatsApp) with: objective, audience, data available, deadline
2. BI evaluates feasibility and classifies complexity
3. Added to backlog with priority score
4. Weekly backlog review with Operations Director and Chief of Staff
5. Delivery with validation session before publication

**Why SLAs mattered:** Before formalization, BI requests arrived via WhatsApp, email, verbal conversation, and hallway encounters — with no tracking, no prioritization, and no accountability. The ANS created a professional intake process and protected the analyst's time for actual build work.

---

## 5 — Operational Procedures

Recurring operational tasks were documented step-by-step so they could be executed by any team member — not only the original BI developer.

**Daily procedures:**
- Validate dataflow execution status in Power BI admin
- Validate dashboard refresh completion
- Cross-check key metrics between Odoo native reports and Power BI
- Respond to any refresh failure alerts

**Monthly procedures:**
- Upload new budget files (commercial and medical services) to Google Sheets
- Update geographic density maps (Python script execution)
- Run RFM segmentation pipeline in correct sequence: `base_segmentation` → `clientes_no_contactar` → `base_RFM`
- Archive previous month's segmentation snapshot before overwriting

**Quarterly procedures:**
- Update ABC product ranking
- Review and update KPI catalog for any new metrics requested

**Event-triggered procedures:**
- New clinic opening → provision new location in Odoo → update clinic dimension table → validate in dashboards
- Staff change in data-owning role → re-train replacement on Google Sheets governance rules
- CData license renewal → coordinate with IT 30 days in advance

---

## 6 — Weekly Governance Cadence

A structured weekly rhythm was established to maintain data quality and stakeholder alignment:

| Meeting | Participants | Purpose |
|---------|-------------|---------|
| Revenue Review | BI + Finance | Reconcile Power BI vs. manual figures · Flag discrepancies |
| Backlog Review | BI + Operations Director + Chief of Staff | Prioritize new requests · Review in-progress work |
| Data Quality Check | BI (solo) | Daily dataflow validation · Source data consistency check |

This rhythm was the operational backbone that made the system trustworthy. The Revenue Review in particular was the mechanism through which Finance certified the numbers — without that certification, dashboard adoption would not have occurred.
