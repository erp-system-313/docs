# Incoterms Moved to Finance — Odoo Alignment

## Implementation: V23 Finance Overhaul

**Date:** 2026-05-15
**Migration:** V23__finance_overhaul.sql
**Commit:** TBD

## Odoo Reference

| Odoo Model | Our Entity | Match |
|---|---|---|
| `account.incoterms` | `Incoterm` | ✅ Full |

## Overview

The `Incoterm` entity (International Commercial Terms) was moved from the Sales module to the Finance module to align with Odoo's architecture where incoterms live under `account.incoterms` rather than under sales.

Incoterms define the responsibilities of buyers and sellers for the delivery of goods under sales contracts (e.g., FOB, CIF, EXW).

## Entity Model

```
Incoterm
├── id
├── code (unique, e.g., "EXW", "FOB", "CIF")
├── name (e.g., "Ex Works", "Free On Board", "Cost Insurance Freight")
└── description
```

## Module Change

| Before | After |
|---|---|
| `com.erp.sales.entity.Incoterm` | `com.erp.finance.entity.Incoterm` |
| `/api/v1/sales/incoterms` | `/api/v1/finance/incoterms` |

## API Endpoints

| Method | Path | Description |
|--------|------|-------------|
| GET | `/api/v1/finance/incoterms` | List all incoterms |
| GET | `/api/v1/finance/incoterms/{id}` | Get incoterm by ID |

## Cross-Module References

- `SalesOrder` still references `Incoterm` via FK but now imports from `com.erp.finance.entity.Incoterm`
- `Move` references `Incoterm` via `incotermId` field (Odoo: `invoice_incoterm_id`)

## Service & Controller

- **Service:** `IncotermService`
- **Controller:** `IncotermController`

## Common Incoterm Codes

| Code | Name | Description |
|---|---|---|
| EXW | Ex Works | Buyer picks up at seller's premises |
| FCA | Free Carrier | Seller delivers to carrier nominated by buyer |
| FOB | Free On Board | Seller loads goods on vessel, risk transfers at ship's rail |
| CIF | Cost, Insurance & Freight | Seller pays cost, insurance, and freight to destination port |
| DAP | Delivered at Place | Seller delivers to named place, ready for unloading |
| DDP | Delivered Duty Paid | Seller delivers with all duties/taxes paid |

## Business Rules

- `code` must be unique (standardized 3-letter codes)
- Incoterms are reference data — typically seeded and rarely modified
- Read-only API (no create/update/delete endpoints) — managed via migration seeds
- Used on both sales orders and invoices to specify delivery terms

## Database Schema

```sql
incoterms (
    id,
    code VARCHAR(10) UNIQUE NOT NULL,
    name VARCHAR(255) NOT NULL,
    description TEXT
)
```
