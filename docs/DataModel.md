
# DataModel.md

## Canonical Data Model

### Jira Core Entities

```
Issue
- issue_key (PK)
- parent_key
- issue_type
- project
- sprint
- assignee
- story_points
- original_estimate

Worklog
- worklog_id (PK)
- issue_key (FK)
- user
- logged_hours
- work_date
```

### External Agile Planning Entities

```
RequestPlan
- sprint
- issue_key
- requested_hours

SprintPlan
- sprint
- issue_key
- planned_hours
- planned_by

CapacityPlan
- sprint
- user
- available_hours
```

### Relationships

- Issue 1‑to‑Many Worklogs
- Sprint 1‑to‑Many Issues
- Issue joins with RequestPlan & SprintPlan
- User joins with CapacityPlan

---
