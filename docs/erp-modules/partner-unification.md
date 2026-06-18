# Partner Unification (res.partner) — Odoo Alignment

## Implementation: V24 Sales Overhaul (enhanced)

**Date:** 2026-05-15
**Migration:** V24__sales_overhaul.sql
**Commit:** TBD

## Odoo Reference

| Odoo Model | Our Entity | Match |
|---|---|---|
| `res.partner` | `Partner` | ✅ Full |

## Overview

The `Partner` entity is a unified contact model that replaces separate customer/vendor entities. Following Odoo's `res.partner` pattern, a single model represents companies, individuals, and contacts with a hierarchical parent/child structure.

### Key Concept: Rank-Based Classification

Instead of separate `is_customer`/`is_vendor` booleans, partner type is determined by rank fields:

| Field | Meaning |
|---|---|
| `customer_rank > 0` | This partner is a customer |
| `supplier_rank > 0` | This partner is a vendor |
| Both `> 0` | Partner is both customer and vendor |
| Both `= 0` | Partner is neither (e.g., a contact person) |

This allows a single entity to serve dual roles and supports the Odoo pattern where ranks can be incremented to track interaction count.

## Entity Model

```
Partner
├── id
├── name
├── type: COMPANY | INDIVIDUAL | CONTACT
├── parent_id → Partner (hierarchical: company → contacts)
├── email
├── phone
├── mobile
├── website
├── tax_id
├── address
├── city
├── state
├── zip_code
├── country
├── is_active
├── credit_limit
├── payment_term_id
├── pricelist_id
├── salesperson_id
├── team_id
├── customer_rank (> 0 = customer)
├── supplier_rank (> 0 = vendor)
├── vat (VAT/Tax ID number)
├── ref (internal reference)
├── account_receivable_id (default receivable account)
├── account_payable_id (default payable account)
├── bank_account_id
├── fiscal_position_id
├── tags (comma-separated)
├── industry_id
├── lang (language code)
├── company_id (multi-company)
├── company_registry
├── notes
├── created_at
└── updated_at
```

## API Endpoints

### Partners

| Method | Path | Description |
|--------|------|-------------|
| GET | `/api/v1/partners` | List all partners (paginated, searchable) |
| GET | `/api/v1/partners/{id}` | Get partner by ID |
| GET | `/api/v1/partners/{id}/contacts` | Get child contacts for a partner |
| POST | `/api/v1/partners` | Create partner |
| PUT | `/api/v1/partners/{id}` | Update partner |
| DELETE | `/api/v1/partners/{id}` | Soft-delete partner (sets is_active = false) |

### Customers

| Method | Path | Description |
|--------|------|-------------|
| POST | `/api/v1/partners/customers` | Create customer (sets customer_rank = 1) |
| GET | `/api/v1/partners/customers` | List all customers (customer_rank > 0) |

### Vendors

| Method | Path | Description |
|--------|------|-------------|
| POST | `/api/v1/partners/vendors` | Create vendor (sets supplier_rank = 1) |
| GET | `/api/v1/partners/vendors` | List all vendors (supplier_rank > 0) |

### Stats

| Method | Path | Description |
|--------|------|-------------|
| GET | `/api/v1/partners/stats/customer-count` | Get total customer count |
| GET | `/api/v1/partners/stats/vendor-count` | Get total vendor count |

## Service & Controller

- **Service:** `PartnerService`
- **Controller:** `PartnerController`

## Service Methods

| Method | Description |
|---|---|
| `createCustomer(CreatePartnerRequest)` | Creates partner and sets `customer_rank = 1` |
| `createVendor(CreatePartnerRequest)` | Creates partner and sets `supplier_rank = 1` |
| `getCustomers()` | Returns all partners where `customer_rank > 0` |
| `getVendors()` | Returns all partners where `supplier_rank > 0` |
| `getCustomerCount()` | Count of partners where `customer_rank > 0` |
| `getVendorCount()` | Count of partners where `supplier_rank > 0` |

## Helper Methods

| Method | Logic |
|---|---|
| `isCustomer()` | `customerRank != null && customerRank > 0` |
| `isVendor()` | `supplierRank != null && supplierRank > 0` |

## Partner Hierarchy

```
Company (type = COMPANY)
├── Contact: John Doe (type = CONTACT, parent_id → Company)
├── Contact: Jane Smith (type = CONTACT, parent_id → Company)
└── Billing Address (type = CONTACT, parent_id → Company)
```

- Companies have `type = COMPANY` and no parent
- Contacts have `type = CONTACT` and reference their parent company via `parent_id`
- Individuals have `type = INDIVIDUAL` and no parent (standalone persons)

## Business Rules

- `customer_rank` and `supplier_rank` default to `0`
- `createCustomer()` sets `customer_rank = 1`; `createVendor()` sets `supplier_rank = 1`
- A partner can be both customer and vendor (both ranks > 0)
- `vat` stores the official VAT/tax identification number
- `ref` is an internal reference code (e.g., legacy system ID)
- `account_receivable_id` and `account_payable_id` default the accounting accounts for this partner
- `tags` is a comma-separated string for categorization
- `lang` is an ISO language code (e.g., "en_US", "fr_FR") for communications
- `company_id` supports multi-company setups
- Soft-delete sets `is_active = false` instead of removing the record

## Database Schema

```sql
partners (
    id,
    name VARCHAR NOT NULL,
    type VARCHAR CHECK (type IN ('COMPANY', 'INDIVIDUAL', 'CONTACT')),
    parent_id FK → partners,
    email VARCHAR,
    phone VARCHAR(20),
    mobile VARCHAR(20),
    website VARCHAR,
    tax_id VARCHAR(50),
    address TEXT,
    city VARCHAR(100),
    state VARCHAR(100),
    zip_code VARCHAR(20),
    country VARCHAR(100),
    is_active BOOLEAN DEFAULT true,
    credit_limit DECIMAL(15,2) DEFAULT 0,
    payment_term_id,
    pricelist_id,
    salesperson_id,
    team_id,
    customer_rank INT DEFAULT 0,
    supplier_rank INT DEFAULT 0,
    vat VARCHAR(50),
    ref VARCHAR(50),
    account_receivable_id,
    account_payable_id,
    bank_account_id,
    fiscal_position_id,
    tags VARCHAR(500),
    industry_id,
    lang VARCHAR(10),
    company_id,
    company_registry VARCHAR(50),
    notes TEXT,
    created_at TIMESTAMP,
    updated_at TIMESTAMP
)
```
