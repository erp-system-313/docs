# Helpdesk Ticket Ratings — Odoo Alignment

## Implementation: V30 Helpdesk Addons

**Date:** 2026-05-15
**Migration:** V30__helpdesk_addons.sql
**Commit:** TBD

## Odoo Reference

| Odoo Model | Our Entity | Match |
|---|---|---|
| `helpdesk.ticket.rating` | `TicketRating` | ✅ Full |

## Entity Model

```
TicketRating
├── id
├── ticket_id → Ticket
├── rating (1-5)
├── comment
├── customer_id
├── customer_name
├── via_email
└── created_at
```

## API Endpoints

| Method | Path | Description |
|--------|------|-------------|
| GET | `/api/v1/support/tickets/{ticketId}/ratings` | List all ratings for a ticket |
| POST | `/api/v1/support/tickets/{ticketId}/ratings` | Submit a rating for a ticket |
| GET | `/api/v1/support/tickets/{ticketId}/ratings/average` | Get average rating for a ticket |
| GET | `/api/v1/support/ratings/overall-average` | Get overall average across all tickets |
| GET | `/api/v1/support/ratings/distribution` | Get rating distribution (count per star level) |
| DELETE | `/api/v1/support/ratings/{id}` | Delete a rating |

## Business Logic

### Rating Submission
1. Customer submits a rating (1-5 stars) for a resolved ticket
2. Optional comment can be included for feedback
3. Rating can be submitted via email (`via_email = true`) or through the portal
4. Only one rating per customer per ticket is allowed

### Average Calculations
- **Ticket Average**: Mean of all ratings for a specific ticket
- **Overall Average**: Mean of all ratings across all tickets
- **Distribution**: Count of ratings grouped by star level (1 through 5)

### Business Rules
- Rating must be between 1 and 5 (inclusive)
- Ticket must exist before a rating can be submitted
- Ratings are immutable after creation (delete and re-create to change)
- Customer name is captured at time of rating for historical accuracy

## Database Schema

```sql
ticket_ratings (
    id,
    ticket_id FK → tickets CASCADE,
    rating INT CHECK (rating >= 1 AND rating <= 5),
    comment TEXT,
    customer_id,
    customer_name VARCHAR,
    via_email BOOLEAN DEFAULT false,
    created_at TIMESTAMP
)
```
