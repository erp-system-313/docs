# Helpdesk Module Overhaul — Implementation Plan

## Objective

Transform the current basic helpdesk CRUD (Ticket + TicketComment) into a full Odoo-inspired helpdesk management system: configurable stages/kanban, teams, SLA policies with deadline tracking, ticket categories and tags, knowledge base, email integration, customer portal access, and a comment management API.

---

## Architecture Pattern

```
Controller (REST) → Service (Business Logic) → Entity (JPA) → Repository (Spring Data)
                                             ↕
                                     DTO (Request/Response)
```

### Key Design Decisions

| Decision | Choice | Rationale |
|----------|--------|-----------|
| Ticket status | **Stages instead of hardcoded enum** — `HelpdeskStage` entity per team with sequence, fold | Configurable kanban stages (Odoo pattern); team-specific stages |
| SLA tracking | **SlaPolicy entity + timer fields** on Ticket — auto-calculated deadlines per priority + team | Automatable escalation; regulatory compliance |
| Knowledge Base | **KbArticle entity** — separate from tickets but linkable | Odoo's `helpdesk.kbase` pattern; reusable solutions |
| CreatedBy tracking | **Add `createdBy` field** to Ticket → User | Missing critical field; who reported the ticket |
| Comment management | **Dedicated service method** `addComment()` — exposed via REST | Currently no API to add comments; only loaded via JPA relation |
| Soft-delete | **`isArchived` boolean** instead of hard delete | Odoo archiving pattern; preserves audit trail |
| Channel tracking | **`channel` enum** on Ticket (email, web, phone, chat) | Know where tickets originate |

---

## Current State Assessment

| Artifact | File | Lines | Status |
|----------|------|-------|--------|
| Ticket entity | `Ticket.java` | 72 | Has hardcoded status enum, no team/category/SLA/createdBy |
| TicketComment entity | `TicketComment.java` | 45 | Has isInternal boolean, no attachment support |
| TicketService | `TicketService.java` | 204 | Basic CRUD, no comment management, no SLA |
| TicketController | `TicketController.java` | 92 | KB endpoint URL is broken (`/api/v1/support/tickets/api/v1/support/kb`) |
| DTOs | 4 files | ~160 | Basic fields only |

### Critical bugs to fix

1. **KB URL mapping broken** — Controller is `@RequestMapping("/api/v1/support/tickets")` but KB endpoint is mapped to `/api/v1/support/kb`, resulting in `/api/v1/support/tickets/api/v1/support/kb`
2. **No comment POST endpoint** — Spec calls for `POST /tickets/{id}/comments` (P2) but not implemented
3. **No `createdBy` tracking** — Ticket entity doesn't track who created it
4. **Hard delete** — `delete()` does hard delete with no archive/undo
5. **`authorName` uses email** — `TicketCommentDto` maps `comment.author.email` instead of user's full name
6. **Null risk** — `assignedTo` name concatenation could produce "null null"

---

## New Entities

### HelpdeskTeam

| Field | Type | Notes |
|-------|------|-------|
| id | Long | PK |
| name | String | NOT NULL |
| description | String | TEXT, nullable |
| members | Set<User> | M2M join table |
| isActive | Boolean | default true |
| createdAt | LocalDateTime | auto |
| updatedAt | LocalDateTime | auto |

**Frontend impact**: New management section. `GET/POST/PUT/DELETE /api/v1/support/teams`.

### HelpdeskStage

| Field | Type | Notes |
|-------|------|-------|
| id | Long | PK |
| name | String | e.g. "New", "In Progress", "Waiting Customer", "Resolved", "Closed" |
| sequence | Integer | Order in kanban |
| fold | Boolean | If true, fold (collapse) in kanban view |
| teamId | Long | FK → HelpdeskTeam (nullable for shared stages) |

**Frontend impact**: Stages replace the hardcoded status dropdown. Kanban view support. `GET/POST /api/v1/support/stages`.

### HelpdeskCategory

| Field | Type | Notes |
|-------|------|-------|
| id | Long | PK |
| name | String | e.g. "Bug", "Feature Request", "Billing" |
| teamId | Long | FK → HelpdeskTeam (nullable) |

**Frontend impact**: Category dropdown on ticket form. `GET/POST /api/v1/support/categories`.

### HelpdeskTag

| Field | Type | Notes |
|-------|------|-------|
| id | Long | PK |
| name | String | NOT NULL |
| color | String | Hex color code, nullable |

**Frontend impact**: Color-coded tags on ticket cards. `GET/POST /api/v1/support/tags`.

### SlaPolicy

| Field | Type | Notes |
|-------|------|-------|
| id | Long | PK |
| name | String | e.g. "Critical 4h", "High 8h" |
| targetStageId | Long | FK → HelpdeskStage (target resolution stage) |
| priority | String | LOW, MEDIUM, HIGH, URGENT |
| deadlineMinutes | Integer | Time in minutes to meet SLA |
| teamId | Long | FK → HelpdeskTeam (nullable) |

**Frontend impact**: SLA deadline displays on ticket. Time remaining countdown.

### KbArticle

| Field | Type | Notes |
|-------|------|-------|
| id | Long | PK |
| title | String | NOT NULL |
| content | String | TEXT, markdown |
| categoryId | Long | FK → HelpdeskCategory (nullable) |
| tags | String | Comma-separated or JSON |
| views | Integer | auto-increment |
| isPublished | Boolean | default false |
| createdBy | Long | FK → User |
| createdAt | LocalDateTime | auto |
| updatedAt | LocalDateTime | auto |

**Frontend impact**: KB browsing/search UI. Rich text editor for articles.

### TicketAttachment

| Field | Type | Notes |
|-------|------|-------|
| id | Long | PK |
| ticketId | Long | FK → Ticket |
| filename | String | Original filename |
| filepath | String | Server storage path |
| mimeType | String | e.g. "image/png", "application/pdf" |
| uploadedBy | Long | FK → User |
| createdAt | LocalDateTime | auto |

**Frontend impact**: File upload on ticket create/reply.

---

## Enhanced Existing Entities

### Ticket (changes)

| Field | Change | Type | Notes |
|-------|--------|------|-------|
| `status` | **Remove enum → replace with stageId** | Long (FK → HelpdeskStage) | Configurable stages instead of hardcoded OPEN/IN_PROGRESS/RESOLVED/CLOSED |
| `teamId` | **New** | Long (FK → HelpdeskTeam) | Team assignment |
| `categoryId` | **New** | Long (FK → HelpdeskCategory) | Ticket category |
| `createdBy` | **New** | Long (FK → User) | Reporter/creator |
| `channel` | **New** | Enum: EMAIL, WEB, PHONE, CHAT | Origination channel |
| `slaDeadline` | **New** | LocalDateTime | Computed from SlaPolicy |
| `slaStatus` | **New** | Enum: OK, BREACHED, PENDING | SLA compliance |
| `closedAt` | **New** | LocalDateTime | When ticket was closed |
| `isArchived` | **New** | Boolean | Soft-delete flag |

**Frontend impact**: Status dropdown replaced by stage selector (kanban). New fields: team, category, channel, SLA timer.

### TicketComment (changes)

| Field | Change | Type | Notes |
|-------|--------|------|-------|
| No structural changes | — | — | `isInternal` boolean already exists for internal vs public notes |

---

## New Services

| Service | Key Methods |
|---------|-------------|
| `HelpdeskTeamService` | `findAll`, `findById`, `create`, `update`, `delete` (soft) |
| `HelpdeskStageService` | `findAll`, `findByTeamId`, `create`, `update`, `reorder` |
| `HelpdeskCategoryService` | `findAll`, `findByTeamId`, `create`, `update` |
| `HelpdeskTagService` | `findAll`, `create`, `delete` |
| `SlaService` | `applySla(ticket)`, `checkDeadlines()`, `escalate(ticket)` |
| `KbService` | `findAll`, `search`, `findById`, `create`, `update`, `delete`, `incrementViews` |
| `TicketService` (enhanced) | `create`, `update`, `addComment`, `changeStage`, `assign`, `escalate`, `merge` |

### SlaService Details

```
applySla(ticket):
  1. Find matching SlaPolicy by (team, priority)
  2. Compute deadline = now + deadlineMinutes
  3. Set ticket.slaDeadline and ticket.slaStatus = PENDING

checkDeadlines():
  1. Find all tickets where slaStatus = PENDING and slaDeadline < now
  2. Set slaStatus = BREACHED
  3. Log escalation event

escalate(ticket):
  1. Auto-assign to team lead
  2. Set priority higher
  3. Create internal note about escalation
```

---

## New/Changed Endpoints

### New endpoints

| Method | Path | Stage | Purpose |
|--------|------|-------|---------|
| GET/POST/PUT/DELETE | `/api/v1/support/teams` | P1 | Helpdesk team CRUD |
| GET/POST/PUT/DELETE | `/api/v1/support/stages` | P1 | Stage CRUD + reorder |
| GET/POST/PUT/DELETE | `/api/v1/support/categories` | P1 | Category CRUD |
| GET/POST/PUT/DELETE | `/api/v1/support/tags` | P1 | Tag CRUD |
| POST | `/api/v1/support/tickets/{id}/comments` | P1 | Add comment to ticket (**currently missing**) |
| POST | `/api/v1/support/tickets/{id}/assign` | P2 | Assign ticket |
| POST | `/api/v1/support/tickets/{id}/stage` | P2 | Change stage |
| GET/POST/PUT/DELETE | `/api/v1/support/kb` | P3 | Knowledge base CRUD (**currently broken URL**) |
| GET | `/api/v1/support/kb/search` | P3 | KB search |
| GET | `/api/v1/support/tickets/stats` | P2 | Dashboard stats (by stage, team, SLA) |

### Changed endpoints

| Method | Path | Change |
|--------|------|--------|
| PUT | `/api/v1/support/tickets/{id}` | `UpdateTicketRequest` now accepts `stageId`, `teamId`, `categoryId`, `channel` instead of old `status` field |

---

## Ticket Stage Transition Rules

```
New → In Progress → Waiting Customer → Resolved → Closed
 ↑         ↑              ↓               ↑
 └─────────┴──────────────┴───────────────┘
            (can reopen/regress)
```

- `New` → any: always allowed
- `In Progress` → `Waiting Customer` or `Resolved`: allowed
- `Waiting Customer` → `In Progress` or `Closed`: allowed (reopen or abandon)
- `Resolved` → `Closed`: allowed (customer confirmed) or → `In Progress` (reopened)
- `Closed` → none (unless stage is configured to allow reopen)

---

## Implementation Phases

| Phase | Steps | Description |
|-------|-------|-------------|
| **A: Foundation** | 1-7 | New entities: HelpdeskTeam, HelpdeskStage, HelpdeskCategory, HelpdeskTag, SlaPolicy, KbArticle, TicketAttachment. Repositories for all. |
| **B: Fixes** | 8-12 | Fix KB URL mapping (separate controller). Add `createdBy` to Ticket. Replace status enum with `stageId`. Add `isArchived` soft-delete. Fix `authorName` to use full name. |
| **C: Services** | 13-17 | HelpdeskTeamService, HelpdeskStageService, CategoryService, TagService, SlaService, KbService. Enhance TicketService (addComment, changeStage, assign). |
| **D: Controllers** | 18-22 | Wire all new endpoints. Fix KB endpoint. Add comment, assign, stage-change endpoints. Add stats endpoint. |
| **E: DTOs** | 23-25 | TicketDto (stageId, teamId, categoryId, channel, sla fields, createdBy, closedAt). CreateTicketRequest (channel, attachments). UpdateTicketRequest (stageId, teamId, categoryId). CommentDto fixes. |
| **F: Migration** | 26 | V25__helpdesk_overhaul.sql — new tables, alter helpdesk_tickets, seed default stages |

---

## Implementation Order (Recommended)

```
Phase A (entities)     → Week 1
Phase B (fixes)        → Week 1 (interleaved)
Phase C (services)     → Week 2
Phase D (controllers)  → Week 2 
Phase E (DTOs)         → Week 2-3
Phase F (migration)    → Week 3
Tests + Polish         → Week 3
```

---

## Frontend Impact Summary

| What changes | Details |
|-------------|---------|
| **Ticket status** | Hardcoded OPEN/IN_PROGRESS/RESOLVED/CLOSED → configurable stages per team. Kanban view support. |
| **New filter fields** | Tickets filterable by team, category, channel, SLA status |
| **Comment API** | `POST /tickets/{id}/comments` — new endpoint to add replies |
| **KB URL fix** | `/api/v1/support/kb` now works (was broken: mapped to `/api/v1/support/tickets/api/v1/support/kb`) |
| **Creator tracking** | Tickets show who created them (`createdBy` field) |
| **SLA timers** | Deadline countdown display, color-coded (OK=green, BREACHED=red) |
| **Attachments** | File upload on ticket create and comment |
| **Stats dashboard** | `GET /api/v1/support/tickets/stats` — counts by stage, team, SLA breaches |
| **Soft-delete** | Tickets are archived, not hard-deleted (`isArchived`). Archived tickets hidden by default. |
