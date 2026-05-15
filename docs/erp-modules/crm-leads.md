# CRM Leads & Pipeline — Odoo Alignment

## Implementation: V32 CRM

**Date:** 2026-05-15
**Migration:** V32__crm.sql
**Commit:** TBD

## Odoo Reference

| Odoo Model | Our Entity | Match |
|---|---|---|
| `crm.lead` | `CrmLead` | ✅ Full |
| `crm.stage` | `CrmLeadStage` | ✅ Full |

## Entity Model

```
CrmLeadStage
├── id
├── name (e.g., "New", "Qualified", "Proposition", "Won")
├── sequence (ordering in pipeline)
├── is_won (marks won stage)
├── is_folded (collapsed in UI)
├── email_template_id (auto-send on entry)
├── team_id (optional, stage specific to team)
└── description

CrmLead
├── id
├── name
├── type: LEAD | OPPORTUNITY
├── stage_id → CrmLeadStage
├── priority (0-3 stars)
├── expected_revenue
├── probability (0-100%)
├── expected_closing (date)
├── partner_id → Partner
├── contact_name
├── contact_email
├── contact_phone
├── source (e.g., "Website", "Referral", "Trade Show")
├── medium (e.g., "Email", "Search", "Social")
├── campaign (e.g., "Summer 2026")
├── user_id (assigned salesperson)
├── team_id (sales team)
├── tags (JSON array)
├── activity_date (next follow-up)
├── lost_reason (when marked lost)
└── converted_at (when lead → opportunity)
```

## API Endpoints

### Leads

| Method | Path | Description |
|--------|------|-------------|
| GET | `/api/v1/crm/leads` | List leads (filtered by type, stage, team, user) |
| GET | `/api/v1/crm/leads/{id}` | Get lead by ID |
| POST | `/api/v1/crm/leads` | Create lead |
| PUT | `/api/v1/crm/leads/{id}` | Update lead |
| DELETE | `/api/v1/crm/leads/{id}` | Delete lead |
| POST | `/api/v1/crm/leads/{id}/mark-won` | Mark opportunity as won |
| POST | `/api/v1/crm/leads/{id}/mark-lost` | Mark opportunity as lost (with reason) |
| POST | `/api/v1/crm/leads/{id}/convert` | Convert lead to opportunity |
| GET | `/api/v1/crm/leads/analytics/pipeline` | Pipeline analytics (by stage) |
| GET | `/api/v1/crm/leads/analytics/revenue` | Revenue analytics (expected vs won) |
| GET | `/api/v1/crm/leads/analytics/conversion` | Conversion rate analytics |

### Stages

| Method | Path | Description |
|--------|------|-------------|
| GET | `/api/v1/crm/stages` | List all stages (ordered by sequence) |
| GET | `/api/v1/crm/stages/{id}` | Get stage by ID |
| POST | `/api/v1/crm/stages` | Create stage |
| PUT | `/api/v1/crm/stages/{id}` | Update stage |
| DELETE | `/api/v1/crm/stages/{id}` | Delete stage |

## Lead vs Opportunity

| | Lead | Opportunity |
|---|---|---|
| **Purpose** | Unqualified prospect | Qualified prospect with revenue potential |
| **Revenue** | Not tracked | Expected revenue + probability |
| **Closing Date** | Not required | Expected closing date |
| **Conversion** | Can be converted to opportunity | Already an opportunity |

## Pipeline Workflow

```
New → Qualified → Proposition → Won
  │       │            │
  │       │            └── Lost (with reason)
  │       └── Convert to Opportunity
  └── Discard
```

### Convert Lead to Opportunity
1. Lead is qualified and has revenue potential
2. System sets `type = OPPORTUNITY`
3. Populates `expected_revenue`, `probability`, `expected_closing`
4. Records `converted_at` timestamp
5. Moves to the first opportunity stage

### Mark as Won
1. Opportunity reaches a stage with `is_won = true`
2. `probability` set to 100%
3. Revenue is recognized as won

### Mark as Lost
1. Opportunity will not close
2. `lost_reason` is required
3. `probability` set to 0%
4. Opportunity moves to a folded/lost stage

## Business Rules

- Probability is auto-set based on stage (configurable per stage)
- Expected revenue = `amount × probability / 100`
- Stages with `is_won = true` automatically set probability to 100%
- Stages with `is_folded = true` are collapsed in the pipeline view
- Email templates trigger automatically when a lead enters a stage with `email_template_id` set
- Tags allow categorization (e.g., "Enterprise", "SMB", "Government")
- Activity date tracks the next follow-up action
- Source/medium/campaign track marketing attribution

## Database Schema

```sql
crm_lead_stages (
    id,
    name VARCHAR,
    sequence INT,
    is_won BOOLEAN DEFAULT false,
    is_folded BOOLEAN DEFAULT false,
    email_template_id,
    team_id FK → teams,
    description TEXT,
    created_at TIMESTAMP,
    updated_at TIMESTAMP
)

crm_leads (
    id,
    name VARCHAR,
    type VARCHAR CHECK (type IN ('LEAD', 'OPPORTUNITY')),
    stage_id FK → crm_lead_stages,
    priority INT CHECK (priority >= 0 AND priority <= 3),
    expected_revenue DECIMAL,
    probability DECIMAL CHECK (probability >= 0 AND probability <= 100),
    expected_closing DATE,
    partner_id FK → partners,
    contact_name VARCHAR,
    contact_email VARCHAR,
    contact_phone VARCHAR,
    source VARCHAR,
    medium VARCHAR,
    campaign VARCHAR,
    user_id FK → users,
    team_id FK → teams,
    tags JSONB,
    activity_date DATE,
    lost_reason TEXT,
    converted_at TIMESTAMP,
    created_at TIMESTAMP,
    updated_at TIMESTAMP
)
```
