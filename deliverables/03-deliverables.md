# Deliverables 03 - Implementation Kickoff

## Technology Stack

| Layer            | Technology            | Status     |
| ---------------- | --------------------- | ---------- |
| **Frontend**     |                       |            |
| Framework        | React 18 + TypeScript | ✅         |
| UI Library       | Ant Design (antd)     | ✅ DECIDED |
| State Management | Context API           | ✅ DECIDED |
| Form Handling    | React Hook Form       | ✅ DECIDED |
| HTTP Client      | Axios                 | ✅         |
| Charts           | Recharts              | ✅         |
| **Backend**      |                       |            |
| Framework        | Spring Boot 3.x       | ✅         |
| Language         | Java 17+              | ✅         |
| Database         | PostgreSQL            | ✅         |
| Auth             | JWT                   | ✅         |
| Security         | Spring Security       | ✅         |

| Pair       | Frontend | Backend | Modules               |
| ---------- | -------- | ------- | --------------------- |
| **Pair A** | FE1      | BE1     | Inventory, Purchasing |
| **Pair B** | FE2      | BE2     | Sales, Finance        |
| **Pair C** | FE3      | BE3     | HR, Admin, Dashboard  |

---

## Part 1: Backend Deliverables

### API Documentation ✅

| Document             | Location               | Status      |
| -------------------- | ---------------------- | ----------- |
| `api/endpoints.md`   | REST API documentation | ✅ Complete |
| `api/data-models.md` | Data models            | ✅ Complete |

### API Conventions

| Convention  | Value                             |
| ----------- | --------------------------------- |
| Versioning  | URL-based (`/v1`)                 |
| Pagination  | `?page=0&size=20`                 |
| Date Format | ISO 8601 (`2026-03-26T10:30:00Z`) |
| ID Format   | Auto-increment integer            |

### Backend Implementation by Pair

#### Pair A (FE1 + BE1) - Inventory & Purchasing

**BE1 responsibilities:**

| Priority | Entity        | Endpoints        | Status  |
| -------- | ------------- | ---------------- | ------- |
| 1        | Product       | CRUD + low-stock | Pending |
| 2        | Category      | CRUD             | Pending |
| 3        | Supplier      | CRUD             | Pending |
| 4        | PurchaseOrder | CRUD + receive   | Pending |
| 5        | StockMovement | CRUD             | Pending |

**Database entities to implement:**

- Product, Category, Supplier, StockMovement
- PurchaseOrder, PurchaseOrderLine

#### Pair B (FE2 + BE2) - Sales & Finance

**BE2 responsibilities:**

| Priority | Entity       | Endpoints                      | Status  |
| -------- | ------------ | ------------------------------ | ------- |
| 1        | Customer     | CRUD                           | Pending |
| 2        | SalesOrder   | CRUD + confirm + ship + cancel | Pending |
| 3        | Invoice      | CRUD + payments + pdf          | Pending |
| 4        | Account      | CRUD                           | Pending |
| 5        | JournalEntry | CRUD + post + reverse          | Pending |

**Database entities to implement:**

- Customer, SalesOrder, SalesOrderLine
- Invoice, Payment
- Account, JournalEntry, JournalEntryLine

#### Pair C (FE3 + BE3) - HR, Admin, Dashboard

**BE3 responsibilities:**

| Priority | Entity       | Endpoints              | Status  |
| -------- | ------------ | ---------------------- | ------- |
| 1        | User + Auth  | Login, refresh, logout | Pending |
| 2        | Dashboard    | Stats                  | Pending |
| 3        | Employee     | CRUD                   | Pending |
| 4        | Attendance   | CRUD + clock-in/out    | Pending |
| 5        | LeaveRequest | CRUD + approve/reject  | Pending |
| 6        | Settings     | CRUD                   | Pending |
| 7        | AuditLog     | List                   | Pending |

**Database entities to implement:**

- User, Role, Employee
- Attendance, LeaveRequest, LeaveBalance
- AuditLog

### Backend Shared (All Backend Devs)

| Task                             | Owner | Status  |
| -------------------------------- | ----- | ------- |
| Database setup (PostgreSQL)      | BE1   | Pending |
| Security (JWT + Spring Security) | BE3   | Pending |
| Global exception handling        | BE1   | Pending |
| Audit logging interceptor        | BE3   | Pending |

### Backend API Summary

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

## Part 2: Frontend Deliverables

### Shared Components (All Frontend Devs)

| Priority | Component          | Owner  | Status  |
| -------- | ------------------ | ------ | ------- |
| 1        | API Client (Axios) | Pair A | Pending |
| 2        | Auth Context       | Pair C | Pending |
| 3        | Sidebar/Navigation | Pair C | Pending |
| 4        | Layout Wrapper     | Pair C | Pending |
| 5        | Toast/Notification | Pair C | Pending |

### Frontend Implementation by Pair

#### Pair A (FE1 + BE1) - Inventory & Purchasing

**FE1 responsibilities:**

| Priority | Page                 | Route                       | Status  |
| -------- | -------------------- | --------------------------- | ------- |
| 1        | Product List         | `/inventory/products`       | Pending |
| 2        | Add Product          | `/inventory/products/new`   | Pending |
| 3        | Categories           | `/inventory/categories`     | Pending |
| 4        | Suppliers List       | `/purchasing/suppliers`     | Pending |
| 5        | Supplier Details     | `/purchasing/suppliers/:id` | Pending |
| 6        | Purchase Order Form  | `/purchasing/orders/new`    | Pending |
| 7        | Purchase Orders List | `/purchasing/orders`        | Pending |

#### Pair B (FE2 + BE2) - Sales & Finance

**FE2 responsibilities:**

| Priority | Page              | Route                   | Status  |
| -------- | ----------------- | ----------------------- | ------- |
| 1        | Customers List    | `/sales/customers`      | Pending |
| 2        | Customer Details  | `/sales/customers/:id`  | Pending |
| 3        | Sales Orders List | `/sales/orders`         | Pending |
| 4        | Sales Order Form  | `/sales/orders/new`     | Pending |
| 5        | Invoice List      | `/finance/invoices`     | Pending |
| 6        | Invoice Form      | `/finance/invoices/new` | Pending |
| 7        | Invoice Details   | `/finance/invoices/:id` | Pending |
| 8        | Journal Entries   | `/finance/journal`      | Pending |
| 9        | Chart of Accounts | `/finance/accounts`     | Pending |

#### Pair C (FE3 + BE3) - HR, Admin, Dashboard

**FE3 responsibilities:**

| Priority | Page             | Route               | Status  |
| -------- | ---------------- | ------------------- | ------- |
| 1        | Login            | `/login`            | Pending |
| 2        | Dashboard        | `/dashboard`        | Pending |
| 3        | Profile          | `/profile`          | Pending |
| 4        | Users List       | `/admin/users`      | Pending |
| 5        | Employee List    | `/hr/employees`     | Pending |
| 6        | Employee Details | `/hr/employees/:id` | Pending |
| 7        | Attendance       | `/hr/attendance`    | Pending |
| 8        | Leave Requests   | `/hr/leave`         | Pending |
| 9        | Company Settings | `/admin/settings`   | Pending |
| 10       | Audit Logs       | `/admin/audit-logs` | Pending |

---

## Development Phases

### Phase 1: Foundation

| Frontend           | Backend        |
| ------------------ | -------------- |
| API Client setup   | Database setup |
| Auth Context       | Security (JWT) |
| Login Page         | Auth endpoints |
| Sidebar Navigation | User CRUD      |

### Phase 2: Core Pages

| Frontend                                | Backend                 |
| --------------------------------------- | ----------------------- |
| Product List + Categories + Add Product | Product + Category CRUD |
| Customers List                          | Customer CRUD           |
| Dashboard basics                        | Dashboard stats         |

### Phase 3: Module Expansion

| Frontend            | Backend            |
| ------------------- | ------------------ |
| Suppliers List      | Supplier CRUD      |
| Sales Order Form    | SalesOrder CRUD    |
| Purchase Order Form | PurchaseOrder CRUD |
| Employee List       | Employee CRUD      |
| Users List          | User management    |

### Phase 4: Integration

| Frontend         | Backend               |
| ---------------- | --------------------- |
| Dashboard (full) | Dashboard aggregation |
| Invoices         | Invoice + Payment     |
| Leave Requests   | Leave + Attendance    |
| Audit Logs       | Audit logging         |

---

## Technology Decisions - All Finalized

| Decision         | Value             | Status     |
| ---------------- | ----------------- | ---------- |
| UI Library       | Ant Design (antd) | ✅ DECIDED |
| State Management | Context API       | ✅ DECIDED |
| Form Handling    | React Hook Form   | ✅ DECIDED |

---

## Previous Deliverables

- [01-deliverables.md](./01-deliverables.md) - UML diagrams
- [02-deliverables.md](./02-deliverables.md) - Shared system diagrams
