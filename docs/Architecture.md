
# Architecture.md

## System Architecture Overview

This document describes the **logical and physical architecture** of the Agile Jira Analytics Platform.

### High‑Level Logical Architecture

```
┌───────────── UI Layer ─────────────┐
│ Query Builder | Visual Builder     │
│ Filters | Drill‑downs | Dashboards │
└───────────────┬───────────────────┘
                │ Query DSL
┌───────────────▼───────────────────┐
│ Agile Semantic Layer               │
│ Measures | Dimensions | Rules      │
└───────────────┬───────────────────┘
                │ Execution Plan
┌───────────────▼───────────────────┐
│ Analytics Engine                   │
│ Planner | SQL Generator | Cache    │
└───────────────┬───────────────────┘
                │
┌───────────────▼───────────────────┐
│ Unified Data Store                 │
│ Jira + External Agile Data         │
└───────────────┬───────────────────┘
                │
┌───────────────▼───────────────────┐
│ Ingestion Layer                    │
│ Jira API | CSV | Excel             │
└───────────────────────────────────┘
```

### Deployment Architecture

- Frontend: SPA (React/Vue)
- Backend: API + Query Engine
- Storage: Postgres + DuckDB
- Scheduler: Background jobs
- Auth: SSO / Token‑based

---
