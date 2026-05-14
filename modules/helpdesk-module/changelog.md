# Helpdesk Module Overhaul — Changelog

## Commits (chronological)

| Commit | Description |
|--------|-------------|
| `488728a` | **DTOs**: Add 8 new DTOs (HelpdeskTeamDto, HelpdeskStageDto, HelpdeskCategoryDto, HelpdeskTagDto, KbArticleDto, CreateHelpdeskTeamRequest, CreateKbArticleRequest, CreateCommentRequest). Enhance 4 existing DTOs (TicketDto, TicketCommentDto, CreateTicketRequest, UpdateTicketRequest) with Odoo-inspired fields (stageId, teamId, categoryId, channel, SLA, tags). |
| `6fa458d` | **Entities + Repositories**: Add 7 new entities (HelpdeskTeam, HelpdeskStage, HelpdeskCategory, HelpdeskTag, SlaPolicy, KbArticle, TicketAttachment) + 7 repositories. Enhance Ticket entity with Odoo fields (stageId, teamId, categoryId, createdBy, channel, slaDeadline, slaStatus, closedAt, isArchived, tags M2M). |
| `eb3adef` | **Services (6 new)**: HelpdeskTeamService (CRUD + member management), HelpdeskStageService (CRUD + reorder + team filtering), HelpdeskCategoryService (CRUD + team filtering), HelpdeskTagService (CRUD), SlaService (applySla, checkDeadlines), KbService (CRUD + search + view counting). |
| `d01e658` | **Enhancements**: Enhanced TicketService (addComment, changeStage, assign, close, soft-delete, SLA integration via SlaService, new field support). Fixed TicketController (removed broken KB endpoint, added POST /{id}/comments, /{id}/assign, /{id}/stage, /{id}/close, GET /stats). Created 5 new controllers: KbController (at /api/v1/support/kb — fixes broken URL), HelpdeskTeamController, HelpdeskStageController, HelpdeskCategoryController, HelpdeskTagController. V25 Flyway migration (8 new tables, alter helpdesk_tickets with 9 new columns, seed default stages + SLA policies). |
| `10d889a` | **Fix**: Correct ApiResponse.error() calls in controllers (code + message both required). |

## Phase Status

| Phase | Description | Status |
|-------|-------------|--------|
| A: Foundation | 7 new entities + 7 repositories + Ticket enhancement | ✅ Done |
| E: DTOs | 8 new DTOs + 4 enhanced | ✅ Done |
| C: Services | 6 new services + TicketService enhancement | ✅ Done |
| D: Controllers | 5 new controllers + TicketController fix | ✅ Done |
| B: Fixes | KB URL fix (separate controller), soft-delete, createdBy, comment API | ✅ Done |
| F: Migration | V25 — new tables, alter tickets, seed default stages + SLA policies | ✅ Done |
