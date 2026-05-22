# 01 — Infrastructure & Tools

## Overview

Building the BI infrastructure was the first and most critical phase. Without a reliable data pipeline, no dashboard would be trustworthy. This section documents every technology decision, the reasoning behind it, and the configuration model implemented.

---

## Stack Implemented

| Component | Technology | Purpose |
|-----------|-----------|---------|
| BI Platform | Power BI Premium (Microsoft 365) | Dashboard development, publishing, and sharing across the organization |
| ERP Connector | CData Power BI Connector for Odoo (ODBC) | Bridge between Odoo ERP and Power BI — enables live and scheduled data extraction |
| Data Gateway | On-premises Standard Gateway (Enterprise Mode) | Connects Power BI Service (cloud) to local/private data sources |
| Report Workspaces | Power BI Premium Workspaces | Isolated environments for production reports, dataflows, and testing |

---

## Gateway Architecture — 2-Node Setup

One of the most critical infrastructure decisions was running a **2-node on-premises gateway** instead of a single machine. This provided redundancy and geographic coverage across two key office locations.

```
┌─────────────────────────┐     ┌──────────────────────────┐
│   Node 1 — Bogotá       │     │   Node 2 — Barranquilla  │
│   (Primary Gateway)     │     │   (Secondary Gateway)    │
│                         │     │                          │
│   Always-on requirement │     │   Regional data access   │
│   Handles Odoo + Sheets │     │   Backup connectivity    │
│   Hourly dataflow runs  │     │                          │
└────────────┬────────────┘     └──────────────┬───────────┘
             │                                 │
             └──────────────┬──────────────────┘
                            ▼
                 Power BI Service (Cloud)
                 Data_Flow_Odoo Workspace
```

**Why Enterprise (Standard) Gateway instead of Personal?**
- Multiple users needed to share the same data connections
- Required DirectQuery compatibility for near-real-time data
- Centralized administration and monitoring through Power BI Admin Portal
- Compatible with Power Apps, Power Automate, and Azure services for future scalability

**Critical operational requirement:** The Bogotá gateway node must remain powered on and connected to the network 24/7. Scheduled dataflow refreshes run hourly starting at 00:30 — any downtime breaks the refresh chain and impacts all dependent dashboards.

---

## CData Connector — Odoo to Power BI

Connecting Power BI to Odoo ERP was the most technically complex piece of the infrastructure. Odoo does not have a native Power BI connector, which required a third-party ODBC bridge.

**Solution chosen:** CData Power BI Connector for Odoo (Starter Edition, annual subscription)

**How it works:**
```
Power BI Desktop / Dataflows
        │
        ▼
CData ODBC Driver (installed on gateway machine)
        │  DSN: "CData Power BI Odoo"
        ▼
Odoo ERP REST API
        │
        ▼
Odoo Database (PostgreSQL backend)
```

**Configuration steps implemented:**
1. Install CData connector on gateway machine
2. Create DSN (Data Source Name) in Windows ODBC Administrator (64-bit)
3. Configure connection: Odoo URL + API token + Database name
4. Copy `.pqx` connector file to Power BI Custom Connectors folder
5. Enable custom connectors in Power BI Desktop security settings
6. Register DSN as a data source in the Power BI Gateway admin panel

**Key technical consideration:** The DSN must be configured in the **same bitness** (32 or 64-bit) as the Power BI Desktop installation. Mismatch causes the connector to be invisible to Power BI — a non-obvious failure mode that requires careful validation.

---

## Power BI Workspace Structure

Two separate workspaces were maintained to separate concerns:

```
Data_Flow_Odoo Workspace
├── Purpose: Centralized dataflow processing
├── Contains: All Power Query (M) transformations
├── Access: BI developers only
└── Outputs: Cleaned, modeled tables consumed by dashboards

MOVET Workspace (Production)
├── Purpose: Published dashboards for end users
├── Contains: 8+ production reports
├── Access: Role-based (executives, managers, clinic staff)
└── Refresh: Staggered after dataflow completion

DATA TESTING Workspace
├── Purpose: Development and QA environment
├── Contains: Beta dashboards, experimental reports
└── Access: BI developer accounts only
```

**Refresh scheduling principle:** Dashboard refresh must always be scheduled *after* its upstream dataflow completes. Example: if `service_orders` dataflow refreshes at 02:00 AM and takes ~15 minutes, the Sales Report dashboard refresh must be set no earlier than 02:30 AM.

---

## License & Cost Management

| Tool | License Type | Renewal Cycle | Critical Note |
|------|-------------|---------------|---------------|
| Power BI Premium | Monthly subscription | Monthly | 2 developer accounts + 2 viewer accounts |
| CData Connector | Annual subscription | Annual | Without renewal, Odoo → Power BI connection breaks entirely |
| On-premises Gateway | Free (included with Power BI) | N/A | Requires dedicated always-on machine |

**Escalability designed in:** The infrastructure was architected to scale from 10 to 27+ clinic locations without changes to the core data model or gateway configuration. New locations only require adding the relevant Odoo source data — no architectural rework needed.

---

## Key Design Decisions

**1. Dataflows as the single source of truth.** Rather than having each dashboard connect directly to Odoo (which would multiply API calls and slow performance), all dashboards consume pre-processed dataflow tables. One transformation, many consumers.

**2. Star schema from day one.** Even with a single developer, enforcing star schema discipline from the beginning made the model extensible. Adding new dimensions (new clinic, new business unit) required only table updates, not model restructuring.

**3. Dedicated always-on machines.** The gateway requirement for 24/7 uptime was communicated formally to IT as a hard dependency — not a preference. SLA documentation (ANS) formalized this as an organizational commitment.

**4. IT collaboration for license auditing.** Participated in a technology license audit with the IT team that identified redundant tools and generated operational cost savings. BI infrastructure decisions were part of the broader organizational technology strategy.
