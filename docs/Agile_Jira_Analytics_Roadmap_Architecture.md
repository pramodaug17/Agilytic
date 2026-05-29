
# Agile Jira Analytics Platform

## Complete Development Roadmap & Architecture

---

## 1. Executive Overview

This document describes the **end‑to‑end roadmap and system architecture** for building a **Jira‑centric Agile Analytics & BI platform** with:

- Dynamic **custom queries (non‑static dashboards)**
- **Jira REST API + CSV/Excel ingestion**
- Effort analysis using **Requested vs Planned vs Actual**
- Native understanding of **Agile / Scrum concepts**
- Drill‑down from Sprint → Epic → Story → Sub‑task → Worklog

Unlike generic BI tools, this platform embeds **Agile semantics** directly into the data and query model.

---

## 2. High‑Level Architecture

```
┌──────────────────────────────────────────────┐
│                  Frontend UI                 │
│  - Custom Query Builder                      │
│  - Visual Builder (Charts, Tables)           │
│  - Filters, Drill‑downs                      │
└───────────────────┬──────────────────────────┘
                    │ (Query DSL / JSON AST)
┌───────────────────▼──────────────────────────┐
│        Agile Semantic & Metrics Layer         │
│  - Agile Measures                            │
│  - Dimensions & Hierarchies                  │
│  - Query Validation                          │
└───────────────────┬──────────────────────────┘
                    │ (Optimized Query Plan)
┌───────────────────▼──────────────────────────┐
│              Analytics Engine                │
│  - Query Planner                             │
│  - SQL Generator                             │
│  - Aggregation & Cache                       │
└───────────────────┬──────────────────────────┘
                    │
┌───────────────────▼──────────────────────────┐
│            Unified Data Store                │
│  - Jira Issues                               │
│  - Jira Worklogs                             │
│  - External Planning / Capacity Data         │
│  - Sprint Snapshots                          │
└───────────────────┬──────────────────────────┘
                    │
┌───────────────────▼──────────────────────────┐
│     Data Ingestion & Normalization            │
│  - Jira REST API Connector                   │
│  - CSV / Excel Upload                        │
│  - Schema Mapping                            │
└──────────────────────────────────────────────┘
```

---

## 3. Canonical Data Model

### 3.1 Jira Data

```
Issue
- issue_key
- parent_key
- issue_type
- project
- sprint
- status
- assignee
- story_points
- original_estimate

Worklog
- issue_key
- user
- logged_hours
- work_date
```

### 3.2 External Agile Planning Data (CSV / Excel)

```
RequestPlan    (Demand requested by product)
- sprint
- issue_key
- requested_hours

SprintPlan     (Sprint commitment)
- sprint
- issue_key
- planned_hours
- planned_by

CapacityPlan   (Team availability)
- sprint
- user
- available_hours
- role
```

These datasets are **joined by Sprint + Issue/User keys**, enabling advanced Agile calculations.

---

## 4. Agile Semantic Layer (Core Intelligence)

### 4.1 Base Measures

```
Requested Effort = Σ RequestPlan.requested_hours
Planned Effort   = Σ SprintPlan.planned_hours
Actual Effort    = Σ Worklog.logged_hours
Capacity         = Σ CapacityPlan.available_hours
```

### 4.2 Derived Agile Measures

```
Requested vs Planned = Requested Effort - Planned Effort
Planned vs Actual    = Planned Effort - Actual Effort
Commitment Ratio     = Planned Effort / Capacity
Utilization %        = Actual Effort / Capacity
Spillover Effort     = Planned - Completed Actual
Scope Creep          = Work added after sprint start
```

These measures are **reusable across all queries and visuals**.

---

## 5. Custom Query Model (Non‑Static)

Users do **not write SQL**. They compose Agile questions using a **Query DSL**.

### Example Query: Sprint Planning (Requested vs Planned)

```json
{
  "dimensions": ["Sprint", "Issue"],
  "measures": ["Requested Effort", "Planned Effort", "Requested vs Planned"],
  "filters": {
    "Sprint": "Sprint 24"
  }
}
```

### Example Query: Sprint Review (Planned vs Actual)

```json
{
  "dimensions": ["Sprint", "Assignee"],
  "measures": ["Planned Effort", "Actual Effort", "Planned vs Actual"]
}
```

The engine validates queries, resolves joins, and generates optimized SQL internally.

---

## 6. Drill‑Down Hierarchy

Default Scrum hierarchy:

```
Sprint
 └─ Epic
     └─ Story
         └─ Sub‑task
             └─ Worklog
```

Drill‑down is achieved by **dimension expansion**, not separate queries.

---

## 7. Visualization Layer

### Supported Visuals

- Bar / Stacked Bar (Planned vs Actual)
- Line (Velocity, Trends)
- Heatmap (Utilization, Capacity)
- Scatter (Effort vs Complexity)
- Tables (Issue / Sub‑task breakdown)

Users can reuse the **same query** with multiple visuals.

---

## 8. Development Roadmap

### Phase 0 – Foundations (2–3 weeks)
- Finalize Agile metrics
- Freeze data contracts
- Define MVP scope

### Phase 1 – Data Ingestion (4–5 weeks)
- Jira REST API sync (issues, worklogs, sprints)
- CSV / Excel upload with mapping UI

### Phase 2 – Data Model (3–4 weeks)
- Normalize Jira and external datasets
- Sprint‑based snapshots

### Phase 3 – Agile Semantic Layer (5–6 weeks)
- Implement measures & derived metrics
- Governance rules

### Phase 4 – Custom Query Engine (4–6 weeks)
- Query DSL
- Validation & planning
- SQL generation

### Phase 5 – Visualization & UI (4–5 weeks)
- Custom query builder
- Visual configuration
- Drill‑down UX

### Phase 6 – Agile UX Presets (3–4 weeks)
- Sprint Planning preset
- Sprint Review preset
- Retrospective analytics

### Phase 7 – Performance & Governance (3 weeks)
- Caching & aggregation
- Access control

### Phase 8 – Sharing & Scale (2–3 weeks)
- Dashboard sharing
- Export & scheduling

**Total Estimated Timeline: 6–8 months**

---

## 9. Recommended Technology Stack

- **Backend**: Node.js or Python
- **Analytics DB**: DuckDB (CSV analytics) + Postgres
- **Frontend**: React / Vue
- **Charts**: Vega‑Lite or ECharts
- **Infra**: Docker, Kubernetes (optional)

---

## 10. Final Positioning

This platform is:

✅ Agile‑native (not generic BI)  
✅ Query‑driven (not static dashboards)  
✅ Jira‑aware by design  
✅ Ideal for Scrum teams & engineering leadership

It intentionally avoids:

❌ Raw SQL exposure  
❌ DAX‑style complexity  
❌ Uncontrolled BI queries

---

## 11. Next Steps

- Finalize MVP features
- Build semantic layer first
- Add UI + visuals afterward
- Expand to Kanban & forecasting in later phases

---

_End of document_
