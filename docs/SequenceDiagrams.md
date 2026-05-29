
# SequenceDiagrams.md

## 1. Jira Data Ingestion

```
User -> UI: Configure Jira Source
UI -> Backend: Save Config
Backend -> Jira API: Fetch Issues
Backend -> Jira API: Fetch Worklogs
Backend -> Storage: Persist Data
```

## 2. Query Execution

```
User -> UI: Build Query
UI -> API: Query DSL
API -> Semantic Layer: Validate
Semantic Layer -> Engine: Plan Query
Engine -> DB: Execute SQL
DB -> Engine: Results
Engine -> UI: Dataset
```

## 3. Drill‑Down Flow

```
User -> UI: Click Sprint
UI -> API: Expand Dimension
API -> Engine: Refine Query
Engine -> DB: Fetch Details
UI: Render Drill‑down View
```

---
