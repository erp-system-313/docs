# Meeting 05 - Module Expansion Planning

**Date:** April 16, 2026
**Attendees:** Development team (6 members)

---

## Summary

Planned expansion of ERP system with 3 new general modules: CRM, Helpdesk, and Project Management.

---

## Decision: Module Scope

| Priority | Module   | Scope            |
| -------- | -------- | ---------------- |
| 1        | CRM      | Base (4 pages)   |
| 2        | Helpdesk | Expand (3 pages) |
| 3        | Project  | Base (3 pages)   |

---

## Module Assignments

### CRM - Pair A

| Component  | Pages           |
| ---------- | --------------- |
| Leads List | `/crm/leads`    |
| Pipeline   | `/crm/pipeline` |
| Dashboard  | `/crm`          |

### Helpdesk - Pair B

| Component      | Pages                  |
| -------------- | ---------------------- |
| Tickets List   | `/support/tickets`     |
| Ticket Details | `/support/tickets/:id` |
| Create Ticket  | `/support/tickets/new` |

### Project - Pair C

| Component       | Pages                 |
| --------------- | --------------------- |
| Projects List   | `/projects`           |
| Project Details | `/projects/:id`       |
| Gantt View      | `/projects/:id/gantt` |

---

## Technical Decisions

- Start with base functionality only (expand later)
- Integration with existing modules confirmed:
  - CRM → Customers, Sales Orders
  - Helpdesk → Customers
  - Project → Employees, Sales

---

## Development Order

1. Backend entities + CRUD APIs
2. Frontend pages (list + details)
3. Test integration
4. Dashboard/Gantt features (if time permits)

---

## Next Steps

1. Review module specifications in 05-deliverables.md
2. Start with highest priority (CRM)
3. Integrate with existing modules
