# Account Move Enhancements — Odoo Alignment

## Implementation: V23 Finance Overhaul (enhanced)

**Date:** 2026-05-15
**Migration:** V23__finance_overhaul.sql
**Commit:** TBD

## Odoo Reference

| Odoo Model | Our Entity | Match |
|---|---|---|
| `account.move` | `Move` | ✅ Full |
| `account.incoterms` | referenced via `incotermId` | ✅ |
| payment registration | `MoveService.registerPayment()` | ✅ Full |

## Overview

The `Move` entity is the central model of the accounting module — serving as both journal entries AND invoices. This unified model follows Odoo's `account.move` design, where `moveType` differentiates the purpose of each record.

### Move Types

| Type | Description | Odoo Equivalent |
|---|---|---|
| `ENTRY` | Miscellaneous journal entry | `entry` |
| `OUT_INVOICE` | Customer invoice | `out_invoice` |
| `IN_INVOICE` | Vendor bill | `in_invoice` |
| `OUT_REFUND` | Customer credit note | `out_refund` |
| `IN_REFUND` | Vendor credit note | `in_refund` |

## Entity Model — New Fields

```
Move (enhanced fields)
├── ... (existing fields)
├── incoterm_id (FK → incoterms, Odoo: invoice_incoterm_id)
├── partner_shipping_id (shipping address partner)
├── invoice_source_email (source email for vendor bills received by email)
└── is_storno (reversal/storno entry flag)
```

## Entity Model — Full Reference

```
Move
├── id
├── name (sequence-driven entry/invoice number)
├── reference (external reference)
├── date (accounting date)
├── state: DRAFT → POSTED → CANCEL
├── move_type: ENTRY | OUT_INVOICE | IN_INVOICE | OUT_REFUND | IN_REFUND
├── journal_id → Journal
├── partner_id
├── partner_name
├── currency_id
├── fiscal_position_id → FiscalPosition
├── payment_term_id → PaymentTerm
├── incoterm_id → Incoterm
├── partner_shipping_id
├── invoice_source_email
├── is_storno
├── invoice_date
├── invoice_date_due
├── invoice_origin
├── invoice_user_id
├── narration
├── amount_untaxed
├── amount_tax
├── amount_total
├── amount_residual
├── amount_total_signed
├── payment_state: NOT_PAID → PARTIAL → PAID
├── reversed_entry_id → Move
├── inalterable_hash
├── secure_sequence_number
├── restrict_mode_hash_table
├── active
├── created_by
├── lines (O2M → MoveLine)
├── created_at
├── updated_at
└── posted_at
```

## API Endpoints

### Moves

| Method | Path | Description |
|--------|------|-------------|
| GET | `/api/v1/finance/moves` | List moves (filtered by state, type, date range) |
| GET | `/api/v1/finance/moves/{id}` | Get move by ID (with lines) |
| POST | `/api/v1/finance/moves/entry` | Create journal entry |
| POST | `/api/v1/finance/moves/invoice` | Create invoice |
| POST | `/api/v1/finance/moves/{id}/post` | Post a draft move |
| POST | `/api/v1/finance/moves/{id}/cancel` | Cancel a move |
| POST | `/api/v1/finance/moves/{id}/reverse` | Reverse a posted move (create credit note) |
| POST | `/api/v1/finance/moves/{id}/payment` | Register a payment against an invoice |

## Payment Registration Workflow

```
Posted Invoice (payment_state = NOT_PAID)
    │
    ▼
registerPayment(amount)
    │
    ├── amount == amount_residual ──→ payment_state = PAID, amount_residual = 0
    ├── amount < amount_residual ──→ payment_state = PARTIAL, amount_residual -= amount
    └── amount > amount_residual ──→ payment_state = PAID, amount_residual = 0 (no overpayment)
```

### Payment State Transitions

| From | To | Condition |
|---|---|---|
| `NOT_PAID` | `PARTIAL` | Payment < total amount |
| `NOT_PAID` | `PAID` | Payment >= total amount |
| `PARTIAL` | `PAID` | Remaining payment covers residual |
| `PARTIAL` | `PARTIAL` | Partial payment still leaves residual |

## Business Rules

- `registerPayment()` only works on moves with `state = POSTED` and `isInvoice() = true`
- `amount_residual` is never allowed to go below zero
- `payment_state` is automatically computed based on residual vs total
- `incoterm_id` links to the Finance module's `Incoterm` entity (moved from Sales)
- `invoice_source_email` tracks the originating email for vendor bills received via email gateway
- `is_storno` flag marks entries created using the Storno (reversal) method — debits become credits and vice versa
- `partner_shipping_id` stores the delivery address for customer invoices
- Reversal entries link back to the original via `reversed_entry_id`

## Move Service Methods

| Method | Description |
|---|---|
| `createEntry(Move)` | Create a miscellaneous journal entry (validates balance) |
| `createInvoice(Move, List<InvoiceLineInput>)` | Create invoice with auto-generated tax and payment term lines |
| `post(Long)` | Post a draft move (validates balance, computes hash) |
| `cancel(Long)` | Cancel a move (not allowed if paid) |
| `reverse(Long)` | Create a reversal/credit note for a posted move |
| `registerPayment(Long, BigDecimal)` | Register payment, reduce residual, update payment state |

## Database Schema

```sql
-- New/enhanced columns on moves table
moves (
    ...
    incoterm_id FK → incoterms,
    partner_shipping_id,
    invoice_source_email VARCHAR(255),
    is_storno BOOLEAN DEFAULT false,
    ...
)
```
