# Helpdesk Timesheets — Odoo Alignment

## Implementation: V30 Helpdesk Addons

**Date:** 2026-05-15
**Migration:** V30__helpdesk_addons.sql
**Commit:** TBD

## Odoo Reference

| Odoo Model | Our Entity | Match |
|---|---|---|
| `helpdesk.timesheet` | `HelpdeskTimesheet` | ✅ Full |

## Entity Model

```
HelpdeskTimesheet
├── id
├── ticket_id → Ticket
├── employee_id
├── employee_name
├── date
├── description
├── unit_amount (hours worked)
├── is_billable
├── so_line_id → SaleOrderLine (optional, for billing)
├── created_by
├── created_at
└── updated_at
```

## API Endpoints

| Method | Path | Description |
|--------|------|-------------|
| GET | `/api/v1/support/tickets/{ticketId}/timesheets` | List all timesheets for a ticket |
| POST | `/api/v1/support/tickets/{ticketId}/timesheets` | Log time on a ticket |
| GET | `/api/v1/support/timesheets/employee/{employeeId}` | Get timesheets by employee |
| GET | `/api/v1/support/tickets/{ticketId}/timesheets/total` | Get total hours spent on a ticket |
| GET | `/api/v1/support/tickets/{ticketId}/timesheets/billable` | Get billable timesheets for a ticket |
| PUT | `/api/v1/support/timesheets/{id}` | Update a timesheet entry |
| DELETE | `/api/v1/support/timesheets/{id}` | Delete a timesheet entry |

## Business Logic

### Time Logging
1. Employee logs time against a ticket with date, description, and duration
2. Timesheet can be marked as billable for invoicing purposes
3. If billable, can be linked to a Sale Order Line (`so_line_id`) for invoicing

### Billable vs Non-Billable
- **Billable** (`is_billable = true`): Time that can be invoiced to the customer, optionally linked to a sale order line
- **Non-Billable** (`is_billable = false`): Internal time tracking, not invoiced

### Aggregation
- **Total Hours**: Sum of all `unit_amount` values for a ticket
- **Billable Hours**: Sum of `unit_amount` where `is_billable = true`

### Business Rules
- `unit_amount` must be positive (hours worked)
- Timesheet date cannot be in the future
- Billable timesheets should link to a valid sale order line for invoicing
- Employee name is captured at time of entry for historical accuracy
- Timesheets can be edited after creation (updated_at tracks modifications)

## Database Schema

```sql
helpdesk_timesheets (
    id,
    ticket_id FK → tickets CASCADE,
    employee_id,
    employee_name VARCHAR,
    date DATE,
    description TEXT,
    unit_amount DECIMAL,
    is_billable BOOLEAN DEFAULT false,
    so_line_id FK → sale_order_lines,
    created_by,
    created_at TIMESTAMP,
    updated_at TIMESTAMP
)
```
