# Analytic Accounting — Odoo Alignment

## Implementation: V27 Analytic Accounting

**Date:** 2026-05-15
**Migration:** V27__analytic_accounting.sql
**Commit:** 2fd2254

## Odoo Reference

| Odoo Model | Our Entity | Match |
|---|---|---|
| `account.analytic.account` | `AnalyticAccount` | ✅ Full |
| `account.analytic.line` | `AnalyticLine` | ✅ Full |
| `account.analytic.plan` | `AnalyticPlan` | ✅ Full |
| `account.analytic.distribution.model` | `AnalyticDistribution` | ✅ Full |

## Entity Model

```
AnalyticPlan
├── id
├── name (e.g., "Departments", "Projects", "Regions")
├── description
├── company_id
├── active
└── accounts → [AnalyticAccount]

AnalyticAccount
├── id
├── name
├── code (unique)
├── plan_id → AnalyticPlan
├── parent_id → AnalyticAccount (self-ref, hierarchy)
├── account_type: EXPENSE | INCOME | NEUTRAL
├── partner_id
├── project_id
├── company_id
├── currency_id
├── group_id
├── tag_ids (JSON)
├── active
├── minimum_balance
├── balance (computed)
├── total_debit (computed)
├── total_credit (computed)
├── start_date
├── end_date
└── manager_id

AnalyticLine
├── id
├── account_id → AnalyticAccount
├── date
├── name
├── move_line_id → MoveLine (source)
├── move_id → Move (source)
├── partner_id
├── product_id
├── quantity
├── amount (negative=cost, positive=revenue)
├── amount_currency
├── currency_id
├── product_uom_id
├── employee_id
├── company_id
└── general_account_id → Account

AnalyticDistribution
├── id
├── source_account_id → AnalyticAccount
├── destination_account_id → AnalyticAccount
├── percentage (0-100)
├── account_type (filter)
├── journal_id → Journal (filter)
├── partner_id (filter)
├── product_id (filter)
├── company_id
└── active
```

## API Endpoints

| Method | Path | Description |
|--------|------|-------------|
| GET | `/api/v1/analytic/plans` | List all plans |
| GET | `/api/v1/analytic/plans/{id}` | Get plan by ID |
| POST | `/api/v1/analytic/plans` | Create plan |
| GET | `/api/v1/analytic/accounts` | List accounts (paginated, filtered) |
| GET | `/api/v1/analytic/accounts/{id}` | Get account by ID |
| GET | `/api/v1/analytic/accounts/root` | Get root accounts (no parent) |
| GET | `/api/v1/analytic/accounts/by-plan/{planId}` | Get accounts by plan |
| POST | `/api/v1/analytic/accounts` | Create account |
| PUT | `/api/v1/analytic/accounts/{id}` | Update account |
| GET | `/api/v1/analytic/accounts/{id}/lines` | Get account lines (date range) |
| GET | `/api/v1/analytic/accounts/{id}/balance` | Get account balance |
| GET | `/api/v1/analytic/balances` | Get all account balances |
| POST | `/api/v1/analytic/lines` | Post single analytic line |
| POST | `/api/v1/analytic/lines/bulk` | Post multiple lines (distribution) |
| GET | `/api/v1/analytic/distributions` | Get applicable distributions |
| POST | `/api/v1/analytic/distributions` | Create distribution rule |

## Use Cases

### 1. Project Cost Tracking
```
Plan: "Projects"
├── Project Alpha (EXPENSE)
│   ├── Development (EXPENSE)
│   └── Testing (EXPENSE)
└── Project Beta (EXPENSE)
```

### 2. Department Budget Tracking
```
Plan: "Departments"
├── Engineering (EXPENSE)
├── Marketing (EXPENSE)
└── Sales (INCOME)
```

### 3. Automatic Cost Distribution
```
Distribution Rule:
- Source: Any expense line
- Destination: Engineering (70%), Management (30%)
- Filter: journal_id = BANK
```

## Seed Data

Migration V27 creates 3 default plans:
1. **Departments** — Track costs by department
2. **Projects** — Track costs by project
3. **Regions** — Track costs by geographic region
