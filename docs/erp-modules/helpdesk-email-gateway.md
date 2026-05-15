# Helpdesk Email Gateway — Odoo Alignment

## Implementation: V33 Email Gateway

**Date:** 2026-05-15
**Migration:** V33__email_gateway.sql
**Commit:** TBD

## Odoo Reference

| Odoo Model | Our Entity | Match |
|---|---|---|
| `mail.alias` | `EmailAlias` | ✅ Full |
| `helpdesk.team` | `HelpdeskTeam` (enhanced) | ✅ Full |
| incoming mail | `IncomingEmail` | ✅ Full |

## Entity Model

```
HelpdeskTeam (enhanced)
├── id
├── name
├── description
├── members (M2M → User)
├── is_active
├── alias_name (e.g., "support", "billing")
├── alias_domain (e.g., "help.example.com")
├── use_alias (enable email alias for this team)
├── default_stage (stage for auto-created tickets)
├── team_lead_id → User
├── auto_assign (auto-assign tickets to team lead)
├── created_at
└── updated_at

EmailAlias
├── id
├── alias_local_part (e.g., "support")
├── alias_domain (e.g., "help.example.com")
├── full_alias (computed: "support@help.example.com")
├── model_type (e.g., "helpdesk.team")
├── alias_defaults (JSON: default values for created records)
├── alias_contact (policy: ANY, PARTNERS, FOLLOWERS, NONE)
├── created_at
└── updated_at

IncomingEmail
├── id
├── message_id (Message-ID header, unique)
├── in_reply_to (In-Reply-To header)
├── from_address
├── subject
├── body_text
├── body_html
├── status: PENDING | PROCESSED | FAILED | REJECTED
├── matched_alias_id → EmailAlias
├── related_ticket_id → Ticket
├── reply_to_ticket_id → Ticket (explicit reply link)
├── created_at
└── updated_at
```

## API Endpoints

### Email Gateway

| Method | Path | Description |
|--------|------|-------------|
| POST | `/api/v1/helpdesk/email/inbound` | Webhook endpoint for incoming emails (from mail server/SES/SendGrid) |
| POST | `/api/v1/helpdesk/email/process-pending` | Trigger processing of pending emails |
| GET | `/api/v1/helpdesk/email/pending` | List pending/unprocessed emails |

### Teams

| Method | Path | Description |
|--------|------|-------------|
| GET | `/api/v1/helpdesk/teams` | List all helpdesk teams |
| POST | `/api/v1/helpdesk/teams` | Create team (with alias configuration) |

### Aliases

| Method | Path | Description |
|--------|------|-------------|
| GET | `/api/v1/helpdesk/aliases` | List all email aliases |
| POST | `/api/v1/helpdesk/aliases` | Create email alias |

## Service & Controller

- **Service:** `EmailGatewayService`
- **Controller:** `EmailGatewayController`

## Email Processing Workflow

```
Inbound Email (webhook)
    │
    ▼
IncomingEmail created (status = PENDING)
    │
    ▼
Match alias (full_alias or alias_local_part + domain)
    │
    ├── No match ──→ REJECTED
    │
    ▼
Check contact policy (alias_contact)
    │
    ├── Policy violated ──→ REJECTED
    │
    ▼
Check for reply (In-Reply-To / References headers or [Ticket#ID] pattern)
    │
    ├── Reply detected ──→ Append to existing ticket (reply_to_ticket_id)
    │
    ▼
Create new ticket with alias defaults
    │
    ▼
Auto-assign if team.auto_assign = true
    │
    ▼
Status = PROCESSED
```

### Reply Detection

1. **In-Reply-To / References headers**: Match `message_id` against previously sent ticket notification emails
2. **Subject pattern**: Parse `[Ticket#ID]` from subject line to find the target ticket
3. If both are present, `In-Reply-To` takes precedence

### Alias Matching

- Incoming email `To:` address is matched against `EmailAlias.full_alias`
- Supports wildcard domain matching if `alias_domain` is configured
- `matched_alias_id` is set on the `IncomingEmail` record for audit trail

### Contact Policy Enforcement

| Policy | Description |
|---|---|
| `ANY` | Anyone can create tickets via this alias |
| `PARTNERS` | Only known partners (existing contacts) |
| `FOLLOWERS` | Only followers of the related model |
| `NONE` | Alias is receive-only (no ticket creation) |

### Auto-Ticket Creation

When a new email creates a ticket:
1. Ticket is assigned to the team linked to the matched alias
2. `default_stage` from the team is applied
3. `alias_defaults` (JSON) are merged into the ticket (e.g., priority, tags, category)
4. If `auto_assign = true` on the team, ticket is assigned to `team_lead_id`

## Business Rules

- `message_id` must be unique (prevents duplicate processing)
- Emails with `status = PROCESSED` are not re-processed
- Failed emails (`status = FAILED`) can be retried via `process-pending`
- `alias_contact` policy is checked before ticket creation
- HTML body is stored for rich-text display; plain text is used for notifications
- Reply emails append to the ticket's message thread, not as new tickets

## Database Schema

```sql
-- Enhanced helpdesk_teams
ALTER TABLE helpdesk_teams ADD COLUMN alias_name VARCHAR(255);
ALTER TABLE helpdesk_teams ADD COLUMN alias_domain VARCHAR(255);
ALTER TABLE helpdesk_teams ADD COLUMN use_alias BOOLEAN DEFAULT false;
ALTER TABLE helpdesk_teams ADD COLUMN default_stage_id FK → helpdesk_stages;
ALTER TABLE helpdesk_teams ADD COLUMN team_lead_id FK → users;
ALTER TABLE helpdesk_teams ADD COLUMN auto_assign BOOLEAN DEFAULT false;

email_aliases (
    id,
    alias_local_part VARCHAR,
    alias_domain VARCHAR,
    full_alias VARCHAR UNIQUE,
    model_type VARCHAR,
    alias_defaults JSONB,
    alias_contact VARCHAR CHECK (alias_contact IN ('ANY', 'PARTNERS', 'FOLLOWERS', 'NONE')),
    created_at TIMESTAMP,
    updated_at TIMESTAMP
)

incoming_emails (
    id,
    message_id VARCHAR UNIQUE,
    in_reply_to VARCHAR,
    from_address VARCHAR,
    subject VARCHAR,
    body_text TEXT,
    body_html TEXT,
    status VARCHAR CHECK (status IN ('PENDING', 'PROCESSED', 'FAILED', 'REJECTED')),
    matched_alias_id FK → email_aliases,
    related_ticket_id FK → tickets,
    reply_to_ticket_id FK → tickets,
    created_at TIMESTAMP,
    updated_at TIMESTAMP
)
```
