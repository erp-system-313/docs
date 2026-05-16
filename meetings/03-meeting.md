# Meeting 03 - API Documentation & Planning

**Date:** March 26, 2026
**Attendees:** Development team (6 members)

---

## Summary

Documented all API endpoints and data models. Finalized API conventions.

---

## API Conventions (Finalized)

| Convention  | Value                             |
| ----------- | --------------------------------- |
| Versioning  | URL-based (`/v1`)                 |
| Pagination  | `?page=0&size=20`                 |
| Date Format | ISO 8601 (`2026-03-26T10:30:00Z`) |
| ID Format   | Auto-increment integer            |

---

## Documentation Created

| Document             | Status                      |
| -------------------- | --------------------------- |
| `api/endpoints.md`   | ✅ Complete (40+ endpoints) |
| `api/data-models.md` | ✅ Complete (18 entities)   |

---

## Endpoints by Module

| Module          | Endpoints                                               |
| --------------- | ------------------------------------------------------- |
| Auth            | login, refresh, logout, forgot-password, reset-password |
| Users           | CRUD, list                                              |
| Dashboard       | stats                                                   |
| Products        | CRUD, low-stock                                         |
| Categories      | CRUD                                                    |
| Suppliers       | CRUD                                                    |
| Sales Orders    | CRUD, confirm, ship, cancel                             |
| Customers       | CRUD, orders                                            |
| Invoices        | CRUD, pdf, payments, send, cancel                       |
| Purchase Orders | CRUD, receive, cancel                                   |
| Accounts        | CRUD                                                    |
| Journal         | CRUD, post, reverse                                     |
| Employees       | CRUD, attendance                                        |
| Attendance      | clock-in, clock-out                                     |
| Leave           | CRUD, approve, reject, balances                         |
| Settings        | get, update                                             |
| Audit Logs      | list                                                    |

---

## Data Models

18 entities documented with fields, types, and constraints:

- Core: User, Role, Employee
- Inventory: Product, Category, Supplier, StockMovement
- Sales: Customer, SalesOrder, SalesOrderLine, Invoice, Payment
- Purchasing: PurchaseOrder, PurchaseOrderLine
- Finance: Account, JournalEntry, JournalEntryLine
- HR: Attendance, LeaveRequest, LeaveBalance
- Admin: AuditLog

---

## Frontend Kickoff Pages

### Shared Components (First)

| Priority | Component          | Used By |
| -------- | ------------------ | ------- |
| 1        | API Client (Axios) | All     |
| 2        | Auth Context       | All     |
| 3        | Sidebar/Navigation | All     |
| 4        | Layout Wrapper     | All     |
| 5        | Toast/Notification | All     |

### Priority Pages by Pair

**Pair A (Inventory & Purchasing):**

1. Product List → 2. Add Product → 3. Categories → 4. Suppliers List → 5. Purchase Order Form

**Pair B (Sales & Customers):**

1. Customers List → 2. Sales Order Form → 3. Sales Orders List → 4. Invoices List → 5. Invoice Form

**Pair C (HR, Admin & Dashboard):**

1. Login → 2. Dashboard → 3. Users List → 4. Employee List → 5. Profile

### Development Phases

- **Phase 1:** Foundation (API Client, Auth, Login, Sidebar)
- **Phase 2:** Core Pages (Product List, Categories, Customers List)
- **Phase 3:** Module Expansion (Suppliers, Sales Orders, Employee List, Users List)
- **Phase 4:** Integration (Dashboard, Invoices, Purchase Orders)

### Technology Stack - All Finalized

| Layer      | Technology                     | Status     |
| ---------- | ------------------------------ | ---------- |
| UI Library | Ant Design (antd)              | ✅ DECIDED |
| State      | Context API                    | ✅ DECIDED |
| Forms      | React Hook Form                | ✅ DECIDED |
| Charts     | Recharts                       | ✅         |
| Backend    | Spring Boot + PostgreSQL + JWT | ✅         |

---

## Deliverables Created

See [03-deliverables.md](../deliverables/03-deliverables.md) for implementation plan.
