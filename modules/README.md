# Module Overhaul Documentation

This directory contains documentation for each module overhaul, structured like an Odoo-inspired implementation plan.

## Structure

Each module has its own subdirectory with:

```
modules/<module-name>/
├── 01-plan.md      ← Architecture, entities, phases, DTO specs
├── changelog.md    ← Chronological commit log
└── work-log.md     ← Session-by-session work notes
```

## Modules

| Module | Status | Team | Description |
|--------|--------|------|-------------|
| [finance-module](finance-module/) | ✅ Complete | — | Double-entry accounting, unified Move model, tax engine, reconciliation, audit trail |
| [sales-module](sales-module/) | ✅ Complete | Pair A | Quotations, pricelists, sales teams, partner model, invoice-from-order |
| [helpdesk-module](helpdesk-module/) | ⬜ Pending (plan ready) | Pair B | SLA, stages, teams, KB, email integration |
| [inventory-module](inventory-module/) | ⬜ Pending | — | Multi-warehouse, lot/serial tracking, valuation, routes |
| [purchasing-module](purchasing-module/) | ⬜ Pending | — | RFQ, purchase agreements, auto-reorder |
| [hr-module](hr-module/) | ⬜ Pending | — | Payroll, recruitment, expenses, timesheets |
| [project-module](project-module/) | ⬜ Pending | — | Profitability, resource planning, milestones |

## Adding a new module

1. Create a subdirectory: `modules/<module-name>/`
2. Add `01-plan.md` following the template structure
3. Add `changelog.md` to track commits
4. Add `work-log.md` for session notes
5. Update this README table

## Template for 01-plan.md

Include these sections:
- **Objective** — what the overhaul achieves
- **Architecture Pattern** — layering, key design decisions
- **New Entities** — table with field names, types, Odoo equivalents
- **Enhanced Existing Entities** — field additions/changes
- **New Services** — service methods and business logic
- **New/Changed Endpoints** — REST API changes
- **Implementation Phases** — ordered steps with dependencies
- **Frontend Impact** — what the UI team needs to know
