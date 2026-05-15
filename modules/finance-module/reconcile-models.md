# Reconcile Models — Odoo Alignment

## Implementation: V28 Reconcile Models

**Date:** 2026-05-15
**Migration:** V28__reconcile_models.sql
**Commit:** 2c83226

## Odoo Reference

| Odoo Model | Our Entity | Match |
|---|---|---|
| `account.reconcile.model` | `ReconcileModel` | ✅ Full |
| `account.reconcile.model.line` | `ReconcileModelLine` | ✅ Full |

## Entity Model

```
ReconcileModel
├── id
├── name
├── description
├── journal_id → Journal (optional filter)
├── rule_type: WRITEOFF_BUTTON | WRITEOFF_SUGGESTION | INVOICE_MATCHING
├── match_partner_name (boolean)
├── match_label (string)
├── match_label_regex (regex pattern)
├── match_notes (string)
├── match_notes_regex (regex pattern)
├── amount_min / amount_max (range filter)
├── require_partner (boolean)
├── require_same_amount (boolean)
├── auto_reconcile (auto-apply without user action)
├── sequence (ordering)
├── active
└── lines → [ReconcileModelLine]

ReconcileModelLine
├── id
├── model_id → ReconcileModel
├── account_id → Account (where to post)
├── label (description for created move line)
├── amount_type: FIXED | PERCENTAGE | BALANCE
├── amount (value or percentage)
├── side: DEBIT | CREDIT
├── tax_id → Tax (optional)
└── sequence
```

## API Endpoints

| Method | Path | Description |
|--------|------|-------------|
| GET | `/api/v1/reconcile-models` | List all active models (with lines) |
| GET | `/api/v1/reconcile-models/{id}` | Get model by ID |
| GET | `/api/v1/reconcile-models/by-journal/{journalId}` | Get models for a journal |
| POST | `/api/v1/reconcile-models` | Create model |
| POST | `/api/v1/reconcile-models/{modelId}/lines` | Add line to model |
| DELETE | `/api/v1/reconcile-models/{id}` | Delete model |

## Use Cases

### 1. Bank Fees Auto-Reconciliation
```
Model: "Bank Fees"
- Match: label contains "FEE" or "CHARGE"
- Amount: 0 to 50
- Auto-reconcile: true
Line:
  - Account: Bank Fees Expense
  - Amount: 100% (BALANCE)
  - Side: DEBIT
```

### 2. Transfer Matching
```
Model: "Internal Transfers"
- Match: label contains "TRANSFER"
- Require same amount: true
- Auto-reconcile: false (WRITEOFF_BUTTON)
Line:
  - Account: Suspense Account
  - Amount: 100% (BALANCE)
  - Side: CREDIT
```
