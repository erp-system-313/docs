# Payment Providers & Transactions — Odoo Alignment

## Implementation: V31 Payment Providers

**Date:** 2026-05-15
**Migration:** V31__payment_providers.sql
**Commit:** TBD

## Odoo Reference

| Odoo Model | Our Entity | Match |
|---|---|---|
| `payment.provider` | `PaymentProvider` | ✅ Full |
| `payment.transaction` | `PaymentTransaction` | ✅ Full |

## Entity Model

```
PaymentProvider
├── id
├── name (e.g., "Stripe", "PayPal")
├── code (unique identifier, e.g., "stripe", "paypal")
├── enabled (can be used for new transactions)
├── state: TEST | PROD
├── api_url
├── public_key
├── secret_key
├── webhook_secret
├── supported_currencies (JSON array)
├── capture_manually (authorize now, capture later)
├── fee_percentage (variable fee, e.g., 2.9)
├── fee_fixed (fixed fee per transaction, e.g., 0.30)
├── return_url
├── cancel_url
├── logo_url
└── description

PaymentTransaction
├── id
├── provider_id → PaymentProvider
├── reference (unique internal reference)
├── amount
├── currency_code (ISO 4217)
├── state: DRAFT → PENDING → AUTHORIZED → CONFIRMED → DONE | CANCELED | ERROR
├── payment_method_type (card, transfer, etc.)
├── provider_reference (ID from payment gateway)
├── card_last_digits
├── card_brand
├── partner_id
├── partner_email
├── invoice_id → Invoice (optional)
├── sale_order_id → SaleOrder (optional)
├── is_capture (true = auto-capture, false = manual)
├── source_transaction_id (for refunds, links to original)
├── provider_message (error/status from gateway)
├── fees_amount (computed from provider fees)
└── settlement_date
```

## API Endpoints

### Payment Providers

| Method | Path | Description |
|--------|------|-------------|
| GET | `/api/v1/payments/providers` | List all providers |
| GET | `/api/v1/payments/providers/{id}` | Get provider by ID |
| POST | `/api/v1/payments/providers` | Create provider |
| PUT | `/api/v1/payments/providers/{id}` | Update provider |
| DELETE | `/api/v1/payments/providers/{id}` | Delete provider |
| POST | `/api/v1/payments/providers/{id}/toggle` | Enable/disable provider |

### Payment Transactions

| Method | Path | Description |
|--------|------|-------------|
| GET | `/api/v1/payments/transactions` | List transactions (filtered) |
| GET | `/api/v1/payments/transactions/{id}` | Get transaction by ID |
| POST | `/api/v1/payments/transactions` | Create transaction |
| POST | `/api/v1/payments/transactions/{id}/authorize` | Authorize payment (hold funds) |
| POST | `/api/v1/payments/transactions/{id}/capture` | Capture authorized funds |
| POST | `/api/v1/payments/transactions/{id}/confirm` | Confirm payment |
| POST | `/api/v1/payments/transactions/{id}/cancel` | Cancel transaction |
| POST | `/api/v1/payments/transactions/{id}/refund` | Refund a completed transaction |
| POST | `/api/v1/payments/webhook/{providerCode}` | Process provider webhook |

## Transaction Lifecycle

### Auto-Capture Flow
```
DRAFT ──create──→ PENDING ──provider confirms──→ DONE
```

### Manual Capture Flow (Authorize & Capture)
```
DRAFT ──authorize──→ AUTHORIZED ──capture──→ DONE
```

### Cancel/Refund Flow
```
DONE ──refund──→ DONE (refund transaction linked via source_transaction_id)
PENDING/AUTHORIZED ──cancel──→ CANCELED
```

## Business Rules

- Provider must be `enabled` to create new transactions
- `reference` must be unique across all transactions
- Refund transactions link to the original via `source_transaction_id`
- Webhook endpoint validates signature using `webhook_secret`
- Fees are computed: `fees_amount = (amount * fee_percentage / 100) + fee_fixed`
- `settlement_date` is populated when funds are settled by the provider
- Card details (`card_last_digits`, `card_brand`) are populated for card payments only
- Transactions linked to invoices or sale orders update their payment state accordingly

## Database Schema

```sql
payment_providers (
    id,
    name VARCHAR,
    code VARCHAR UNIQUE,
    enabled BOOLEAN DEFAULT true,
    state VARCHAR CHECK (state IN ('TEST', 'PROD')),
    api_url VARCHAR,
    public_key TEXT,
    secret_key TEXT,
    webhook_secret TEXT,
    supported_currencies JSONB,
    capture_manually BOOLEAN DEFAULT false,
    fee_percentage DECIMAL DEFAULT 0,
    fee_fixed DECIMAL DEFAULT 0,
    return_url VARCHAR,
    cancel_url VARCHAR,
    logo_url VARCHAR,
    description TEXT,
    created_at TIMESTAMP,
    updated_at TIMESTAMP
)

payment_transactions (
    id,
    provider_id FK → payment_providers,
    reference VARCHAR UNIQUE,
    amount DECIMAL,
    currency_code VARCHAR(3),
    state VARCHAR CHECK (state IN ('DRAFT', 'PENDING', 'AUTHORIZED', 'CONFIRMED', 'DONE', 'CANCELED', 'ERROR')),
    payment_method_type VARCHAR,
    provider_reference VARCHAR,
    card_last_digits VARCHAR(4),
    card_brand VARCHAR,
    partner_id,
    partner_email VARCHAR,
    invoice_id FK → invoices,
    sale_order_id FK → sale_orders,
    is_capture BOOLEAN DEFAULT true,
    source_transaction_id FK → payment_transactions,
    provider_message TEXT,
    fees_amount DECIMAL,
    settlement_date DATE,
    created_at TIMESTAMP,
    updated_at TIMESTAMP
)
```
