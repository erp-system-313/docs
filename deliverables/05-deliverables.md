# Deliverables 05 - Module Expansion (CRM, Helpdesk, Project)

## Overview

Phase 5 adds 3 general modules: CRM, Helpdesk, and Project Management. This phase starts with a minimal base and expands over time.

---

## Technology Stack

Same as Phase 3:

- Frontend: React 18 + TypeScript + Ant Design
- Backend: Spring Boot 3.x + PostgreSQL + JWT

---

## Module 1: CRM (Minimal)

### Frontend Pages (Base: 4 pages)

| Page          | Route            | Priority | Description             |
| ------------- | ---------------- | -------- | ----------------------- |
| CRM Dashboard | `/crm`           | P1       | Pipeline overview, KPIs |
| Leads List    | `/crm/leads`     | P1       | Lead management table   |
| Pipeline      | `/crm/pipeline`  | P1       | Kanban view             |
| Lead Details  | `/crm/leads/:id` | P2       | Lead view, convert      |

### Backend Entities (Base: 3)

| Entity        | Priority | Fields                              |
| ------------- | -------- | ----------------------------------- |
| Lead          | P1       | name, email, phone, company, status |
| Opportunity   | P1       | customer_id, stage, revenue         |
| PipelineStage | P1       | name, sequence                      |

### Backend Endpoints (Base: 8)

| Method | Endpoint                       | Priority |
| ------ | ------------------------------ | -------- |
| GET    | `/crm/dashboard`               | P1       |
| GET    | `/crm/leads`                   | P1       |
| POST   | `/crm/leads`                   | P1       |
| GET    | `/crm/leads/:id`               | P1       |
| PUT    | `/crm/leads/:id`               | P2       |
| POST   | `/crm/leads/:id/convert`       | P2       |
| GET    | `/crm/pipelines`               | P1       |
| PUT    | `/crm/opportunities/:id/stage` | P1       |

**Integration:** Links to Customers, Sales Orders

---

## Module 2: Helpdesk (Expand Existing)

### Frontend Pages (Base: 3 pages)

| Page           | Route                  | Priority | Description |
| -------------- | ---------------------- | -------- | ----------- |
| Tickets List   | `/support/tickets`     | P1       | All tickets |
| Ticket Details | `/support/tickets/:id` | P1       | Ticket view |
| Create Ticket  | `/support/tickets/new` | P2       | New ticket  |

### Backend Entities (Base: 2)

| Entity        | Priority | Fields                                            |
| ------------- | -------- | ------------------------------------------------- |
| Ticket        | P1       | title, description, customer_id, priority, status |
| TicketComment | P2       | ticket_id, message                                |

### Backend Endpoints (Base: 6)

| Method | Endpoint                        | Priority |
| ------ | ------------------------------- | -------- |
| GET    | `/support/tickets`              | P1       |
| POST   | `/support/tickets`              | P1       |
| GET    | `/support/tickets/:id`          | P1       |
| PUT    | `/support/tickets/:id`          | P2       |
| POST   | `/support/tickets/:id/comments` | P2       |
| GET    | `/support/kb`                   | P3       |

**Integration:** Links to Customers

---

## Module 3: Project Management (Minimal)

### Frontend Pages (Base: 3 pages)

| Page            | Route                 | Priority | Description       |
| --------------- | --------------------- | -------- | ----------------- |
| Projects List   | `/projects`           | P1       | All projects      |
| Project Details | `/projects/:id`       | P1       | Tasks, timesheets |
| Gantt View      | `/projects/:id/gantt` | P2       | Timeline          |

### Backend Entities (Base: 3)

| Entity    | Priority | Fields                                                 |
| --------- | -------- | ------------------------------------------------------ |
| Project   | P1       | name, customer_id, date_start, date_end, budget, state |
| Task      | P1       | project_id, name, stage, assigned_to                   |
| TaskStage | P1       | name, sequence                                         |

### Backend Endpoints (Base: 7)

| Method | Endpoint              | Priority |
| ------ | --------------------- | -------- |
| GET    | `/projects`           | P1       |
| POST   | `/projects`           | P1       |
| GET    | `/projects/:id`       | P1       |
| GET    | `/projects/:id/tasks` | P1       |
| POST   | `/projects/:id/tasks` | P1       |
| PUT    | `/tasks/:id`          | P2       |
| GET    | `/projects/:id/gantt` | P2       |

**Integration:** Links to Employees, Sales (for project-based billing)

---

## Summary by Pair

### Pair A: CRM Focus

| Task            | Type | Priority |
| --------------- | ---- | -------- |
| Lead entities   | BE   | P1       |
| Lead CRUD APIs  | BE   | P1       |
| Leads List page | FE   | P1       |
| Pipeline page   | FE   | P1       |

### Pair B: Helpdesk Focus

| Task                | Type | Priority |
| ------------------- | ---- | -------- |
| Ticket entities     | BE   | P1       |
| Ticket CRUD APIs    | BE   | P1       |
| Tickets List page   | FE   | P1       |
| Ticket Details page | FE   | P1       |

### Pair C: Project Focus

| Task                 | Type | Priority |
| -------------------- | ---- | -------- |
| Project entities     | BE   | P1       |
| Project CRUD APIs    | BE   | P1       |
| Projects List page   | FE   | P1       |
| Project Details page | FE   | P1       |

---

## Phase Summary

| Module         | FE Pages | BE Entities | BE Endpoints |
| -------------- | -------- | ----------- | ------------ |
| CRM            | 4        | 3           | 8            |
| Helpdesk       | 3        | 2           | 6            |
| Project        | 3        | 3           | 7            |
| **Total Base** | **10**   | **8**       | **21**       |

---

## Previous Deliverables

- [01-deliverables.md](./01-deliverables.md) - UML diagrams
- [02-deliverables.md](./02-deliverables.md) - Shared system diagrams
- [03-deliverables.md](./03-deliverables.md) - Implementation foundation
- [04-deliverables.md](./04-deliverables.md) - Development integration

---

## Status

| Module        | Phase | Status                    |
| ------------- | ----- | ------------------------- |
| Core ERPs (7) | 01-04 | ✅ Implemented            |
| CRM           | 05    | 🚧 In Progress            |
| Helpdesk      | 05    | ✅ Implemented (see note) |
| Project       | 05    | 🚧 In Progress            |
