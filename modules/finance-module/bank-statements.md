# Bank Statements — Odoo Alignment

## Implementation: V26 Bank Statements

**Date:** 2026-05-15
**Migration:** V26__bank_statements.sql
**Commit:** b5388b0

## Odoo Reference

| Odoo Model | Our Entity | Match |
|---|---|---|
| `account.bank.statement` | `BankStatement` | ✅ Full |
| `account.bank.statement.line` | `BankStatementLine` | ✅ Full |

## Entity Model

```
BankStatement
├── id
├── name (auto-generated: JOURNAL/STMT/0001)
├── reference (bank reference)
├── journal_id → Journal (BANK type)
├── bank_account_id → Account (liquidity account)
├── state: DRAFT → OPEN → VALIDATED → RECONCILED → CLOSED
├── balance_start (opening balance)
├── balance_end_real (from bank)
├── balance_end (computed: start + sum of lines)
├── difference (end_real - end)
├── date
├── date_done
├── lines → [BankStatementLine]
└── created_by

BankStatementLine
├── id
├── statement_id → BankStatement
├── sequence
├── date (transaction date)
├── description
├── payment_reference (SEPA ref)
├── partner_id
├── partner_name
├── amount (positive=deposit, negative=withdrawal)
├── account_id (liquidity account)
├── counterpart_account_id (where money goes/comes from)
├── move_id → Move (created on reconciliation)
├── is_reconciled
├── import_id (for OFX/CAMT imports)
└── transaction_type
```

## State Machine

```
DRAFT ──confirm──→ OPEN ──validate──→ VALIDATED ──close──→ CLOSED
  ↑                  │                    │
  └──reopen──────────┘                    │
                                          │
                              (all lines reconciled)
                                          ↓
                                    RECONCILED ──close──→ CLOSED
```

## API Endpoints

| Method | Path | Description |
|--------|------|-------------|
| GET | `/api/v1/bank-statements` | List with filters (journal, state, date range) |
| GET | `/api/v1/bank-statements/{id}` | Get statement with lines |
| GET | `/api/v1/bank-statements/{id}/lines` | Get all lines |
| POST | `/api/v1/bank-statements` | Create statement (with optional lines) |
| POST | `/api/v1/bank-statements/{id}/lines` | Add line to statement |
| DELETE | `/api/v1/bank-statements/{sid}/lines/{lid}` | Remove line (draft only) |
| POST | `/api/v1/bank-statements/{id}/confirm` | DRAFT → OPEN |
| POST | `/api/v1/bank-statements/{id}/validate` | OPEN → VALIDATED (all lines reconciled) |
| POST | `/api/v1/bank-statements/{id}/close` | VALIDATED/RECONCILED → CLOSED |
| POST | `/api/v1/bank-statements/{id}/reopen` | Back to DRAFT |
| POST | `/api/v1/bank-statements/{sid}/lines/{lid}/reconcile` | Reconcile line → creates Move |

## Reconciliation Flow

1. **Import/Create** bank statement with lines
2. **Confirm** statement (DRAFT → OPEN)
3. **Reconcile each line** — system creates a Move with:
   - Liquidity line (bank account debit/credit)
   - Counterpart line (receivable/payable/expense account)
4. **Validate** when all lines reconciled (OPEN → VALIDATED)
5. **Close** to finalize (VALIDATED → CLOSED)

## Business Rules

- Lines can only be added to DRAFT or OPEN statements
- Lines can only be removed from DRAFT statements
- Validation requires ALL lines to be reconciled
- Closed statements cannot be reopened
- Balance end is auto-computed: `balance_start + Σ(line amounts)`
- Difference shows gap between bank balance and computed balance

## Database Schema

```sql
bank_statements (
    id, name UNIQUE, reference, journal_id FK, bank_account_id FK,
    state, balance_start, balance_end_real, balance_end, difference,
    date, date_done, created_by, created_at, updated_at
)

bank_statement_lines (
    id, statement_id FK CASCADE, sequence, date, description,
    payment_reference, partner_id, partner_name, amount,
    account_id FK, counterpart_account_id FK, move_id FK,
    is_reconciled, import_id, transaction_type, created_at
)
```
