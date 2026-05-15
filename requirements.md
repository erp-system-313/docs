# ERP System — Comprehensive Requirements Document

## 1. Project Overview

A unified ERP platform for small-to-medium enterprises to streamline daily operations, improve data visibility, and support decision-making. The system follows a modular architecture with 10 modules, a React frontend, and a Spring Boot backend.

### 1.1 Business Goals

- Centralize core business data (inventory, sales, purchasing, finance, HR)
- Automate workflows (order processing, leave approval, invoice-to-payment)
- Provide real-time dashboards and reports for decision-making
- Enable role-based access for admins, managers, and staff
- Scale horizontally with stateless design and caching

### 1.2 Target Users

| Role    | Access                                                      |
| ------- | ----------------------------------------------------------- |
| Admin   | Full system access, user management, audit logs             |
| Manager | Module-level access, approvals (leave, purchase orders)     |
| Staff   | Day-to-day operations (sales orders, inventory, attendance) |

---

## 2. Technology Stack

### 2.1 Backend

| Layer      | Technology                                      | Purpose                     |
| ---------- | ----------------------------------------------- | --------------------------- |
| Framework  | Spring Boot 3.x                                 | REST API                    |
| Language   | Java 21                                         | Backend language            |
| Build      | Maven 3.9+                                      | Dependency/build management |
| ORM        | Spring Data JPA (Hibernate)                     | Database abstraction        |
| Database   | PostgreSQL 15                                   | Primary data store          |
| Cache      | Redis 7                                         | Session & data caching      |
| Migrations | Flyway                                          | Versioned DB schema changes |
| Auth       | Spring Security + JWT (access + refresh tokens) | AuthN/AuthZ                 |
| Validation | Hibernate Validator (`jakarta.validation`)      | Request validation          |
| API Docs   | SpringDoc OpenAPI                               | Auto-generated API docs     |
| Testing    | JUnit 5 + Mockito                               | Unit & integration tests    |

### 2.2 Frontend

| Layer       | Technology                     | Purpose                    |
| ----------- | ------------------------------ | -------------------------- |
| Framework   | React 19                       | UI library                 |
| Language    | TypeScript                     | Type safety                |
| Build       | Vite                           | Bundler/dev server         |
| UI Library  | Ant Design 6                   | Component library          |
| State       | Context API                    | Global state (auth, theme) |
| Routing     | React Router v7                | Client-side routing        |
| HTTP Client | Axios                          | API communication          |
| Charts      | Recharts                       | Data visualization         |
| Forms       | Ant Design Form + validation   | Form handling              |
| Testing     | Vitest + React Testing Library | Unit tests                 |

### 2.3 DevOps & Infrastructure

| Tool           | Purpose                                         |
| -------------- | ----------------------------------------------- |
| Docker         | Containerization                                |
| Docker Compose | Local PostgreSQL + Redis                        |
| Nix Flakes     | Reproducible dev environment                    |
| Git            | Version control                                 |
| GitHub         | Repository hosting, issue tracking, PR workflow |

---

## 3. System Architecture

### 3.1 High-Level Architecture

```
[Browser] → [React SPA]
                ↓  HTTPS + JWT
         [Spring Boot REST API]
                ↓        ↓
         [PostgreSQL]  [Redis]
```

### 3.2 Backend Layered Architecture

```
Controller Layer  ←  REST endpoints, DTO validation
    ↓
Service Layer     ←  Business logic, transactions, @Auditable
    ↓
Repository Layer  ←  JPA data access
    ↓
Entity Layer      ←  JPA entities ↔ SQL tables
```

### 3.3 Frontend Architecture

```
Page Components  ←  Route-level pages
    ↓
Service Layer    ←  API calls (axios)
    ↓
Context/Hooks    ←  State management
    ↓
Common Components ←  DataTable, FormField, StatusBadge, etc.
```

### 3.4 Request Lifecycle

1. User triggers action → React component → service layer
2. Axios sends HTTP request with JWT in `Authorization` header
3. Backend security filter validates JWT → sets security context
4. Controller routes to service method
5. Service executes business logic (with transactions)
6. Repository queries/updates database
7. Response flows back: entity → DTO → JSON → React state → UI update

---

## 4. Module Catalog

### 4.1 Module Summary

| #   | Module     | Status                 | BE Entities                                                                                             | BE Endpoints | FE Pages | FE Routes       |
| --- | ---------- | ---------------------- | ------------------------------------------------------------------------------------------------------- | ------------ | -------- | --------------- |
| 1   | Auth       | ✅ Done                | User, Role                                                                                              | 4            | 2        | `/login`        |
| 2   | Dashboard  | ✅ Done                | -                                                                                                       | 1            | 1        | `/dashboard`    |
| 3   | Admin      | ✅ Done                | User, Role, AuditLog, Settings                                                                          | 10           | 4        | `/admin/*`      |
| 4   | HR         | ✅ Done                | Employee, Attendance, LeaveRequest, LeaveBalance                                                        | 14           | 4        | `/hr/*`         |
| 5   | Inventory  | ✅ Done                | Product, Category, StockMovement                                                                        | 12           | 5        | `/inventory/*`  |
| 6   | Sales      | ✅ Done                | SalesOrder, SalesOrderLine, Customer                                                                    | 12           | 5        | `/sales/*`      |
| 7   | Purchasing | ✅ Done                | PurchaseOrder, PurchaseOrderLine, Supplier                                                              | 8            | 4        | `/purchasing/*` |
| 8   | Finance    | ✅ Done                | Account, JournalEntry, JournalEntryLine, Invoice, Payment                                               | 14           | 5        | `/finance/*`    |
| 9   | Project    | ✅ Done                | Project, Task, TaskStage                                                                                | 10           | 3        | `/projects/*`   |
| 10  | CRM        | 🚧 FE done, BE missing | Lead, Opportunity, PipelineStage                                                                        | 8            | 4        | `/crm/*`        |
| 11  | Helpdesk   | ✅ Done                | Ticket, TicketComment, HelpdeskStage, HelpdeskTeam, HelpdeskCategory, HelpdeskTag, SlaPolicy, KbArticle | 10           | 4        | `/support/*`    |

**Total:** 11 modules planned, 8 fully implemented, 2 partially, 1 not started.

### 4.2 Module Details

#### 4.2.1 Auth Module

**Purpose:** User authentication, registration, password management.

| Endpoint                       | Method | Description                                 |
| ------------------------------ | ------ | ------------------------------------------- |
| `/api/v1/auth/login`           | POST   | Login with email/password, returns JWT pair |
| `/api/v1/auth/register`        | POST   | Register new user (admin only)              |
| `/api/v1/auth/refresh`         | POST   | Refresh access token                        |
| `/api/v1/auth/logout`          | POST   | Invalidate refresh token                    |
| `/api/v1/auth/me`              | GET    | Get current authenticated user              |
| `/api/v1/auth/forgot-password` | POST   | Request password reset                      |
| `/api/v1/auth/reset-password`  | POST   | Reset password with token                   |

**Entities:** User (id, email, passwordHash, roleId, employeeId, isActive), Role (id, name, permissions)

**Frontend Pages:** Login (`/login`), Profile (`/profile`)

---

#### 4.2.2 Dashboard Module

**Purpose:** Real-time overview of key business metrics.

| Endpoint                  | Method | Description                                               |
| ------------------------- | ------ | --------------------------------------------------------- |
| `/api/v1/dashboard/stats` | GET    | Aggregated stats (sales, pending orders, low stock, etc.) |

**Frontend Pages:** Dashboard (`/dashboard`) — stat cards, sales trend chart, top products, recent orders

---

#### 4.2.3 Admin Module

**Purpose:** User & role management, company settings, audit trail.

| Endpoint             | Method | Description                             |
| -------------------- | ------ | --------------------------------------- |
| `/api/v1/users`      | GET    | List users (paginated, filterable)      |
| `/api/v1/users/{id}` | GET    | Get user                                |
| `/api/v1/users`      | POST   | Create user                             |
| `/api/v1/users/{id}` | PUT    | Update user                             |
| `/api/v1/users/{id}` | DELETE | Soft-delete user                        |
| `/api/v1/settings`   | GET    | Get company settings                    |
| `/api/v1/settings`   | PUT    | Update company settings                 |
| `/api/v1/audit-logs` | GET    | List audit logs (paginated, filterable) |

**Entities:** User, Role, AuditLog, Settings
**Frontend Pages:** User Management (`/admin/users`), Settings (`/admin/settings`), Audit Logs (`/admin/audit-logs`)

**Roles:** ADMIN, USER (configurable via Role entity with JSON permissions)

---

#### 4.2.4 HR Module

**Purpose:** Employee records, attendance tracking, leave management.

| Endpoint                              | Method | Description                       |
| ------------------------------------- | ------ | --------------------------------- |
| `/api/v1/employees`                   | GET    | List employees                    |
| `/api/v1/employees/{id}`              | GET    | Get employee                      |
| `/api/v1/employees`                   | POST   | Create employee                   |
| `/api/v1/employees/{id}`              | PUT    | Update employee                   |
| `/api/v1/employees/{id}`              | DELETE | Terminate employee                |
| `/api/v1/employees/{id}/attendance`   | GET    | Get attendance for employee       |
| `/api/v1/attendance`                  | GET    | List attendance records           |
| `/api/v1/attendance/clock-in`         | POST   | Clock in                          |
| `/api/v1/attendance/clock-out`        | POST   | Clock out                         |
| `/api/v1/leave-requests`              | GET    | List leave requests (filterable)  |
| `/api/v1/leave-requests/{id}`         | GET    | Get leave request                 |
| `/api/v1/leave-requests`              | POST   | Submit leave request              |
| `/api/v1/leave-requests/{id}/approve` | PUT    | Approve leave                     |
| `/api/v1/leave-requests/{id}/reject`  | PUT    | Reject leave                      |
| `/api/v1/leave-balances`              | GET    | Get current user's leave balances |

**Entities:** Employee, Attendance, LeaveRequest, LeaveBalance

**Leave Types:** ANNUAL, SICK, PERSONAL, UNPAID
**Leave Statuses:** PENDING, APPROVED, REJECTED, CANCELLED

**Frontend Pages:** Employees (`/hr/employees`, `/hr/employees/:id`), Attendance (`/hr/attendance`), Leave (`/hr/leave`)

---

#### 4.2.5 Inventory Module

**Purpose:** Product catalog, categories, stock tracking.

| Endpoint                     | Method | Description                       |
| ---------------------------- | ------ | --------------------------------- |
| `/api/v1/products`           | GET    | List products                     |
| `/api/v1/products/{id}`      | GET    | Get product                       |
| `/api/v1/products/low-stock` | GET    | Low stock alerts                  |
| `/api/v1/products`           | POST   | Create product                    |
| `/api/v1/products/{id}`      | PUT    | Update product                    |
| `/api/v1/products/{id}`      | DELETE | Soft-delete product               |
| `/api/v1/categories`         | GET    | List categories                   |
| `/api/v1/categories/{id}`    | GET    | Get category                      |
| `/api/v1/categories`         | POST   | Create category                   |
| `/api/v1/categories/{id}`    | PUT    | Update category                   |
| `/api/v1/categories/{id}`    | DELETE | Delete category                   |
| `/api/v1/stock-movements`    | GET    | List stock movements (filterable) |

**Entities:** Product, Category, StockMovement

**Stock Movement Types:** IN, OUT, ADJUSTMENT

**Frontend Pages:** Overview (`/inventory`), Products (`/inventory/products`, `/inventory/products/:id`, `/inventory/products/new`, `/inventory/products/:id/edit`), Categories (`/inventory/categories`)

---

#### 4.2.6 Sales Module

**Purpose:** Sales order management, customer management.

| Endpoint                            | Method | Description          |
| ----------------------------------- | ------ | -------------------- |
| `/api/v1/sales-orders`              | GET    | List sales orders    |
| `/api/v1/sales-orders/{id}`         | GET    | Get sales order      |
| `/api/v1/sales-orders`              | POST   | Create sales order   |
| `/api/v1/sales-orders/{id}`         | PUT    | Update sales order   |
| `/api/v1/sales-orders/{id}/confirm` | PUT    | Confirm order        |
| `/api/v1/sales-orders/{id}/ship`    | PUT    | Mark shipped         |
| `/api/v1/sales-orders/{id}/cancel`  | PUT    | Cancel order         |
| `/api/v1/sales-orders/{id}`         | DELETE | Delete draft order   |
| `/api/v1/customers`                 | GET    | List customers       |
| `/api/v1/customers/{id}`            | GET    | Get customer         |
| `/api/v1/customers/{id}/orders`     | GET    | Get customer orders  |
| `/api/v1/customers`                 | POST   | Create customer      |
| `/api/v1/customers/{id}`            | PUT    | Update customer      |
| `/api/v1/customers/{id}`            | DELETE | Soft-delete customer |

**Entities:** SalesOrder, SalesOrderLine, Customer

**Order Statuses:** DRAFT, CONFIRMED, SHIPPED, INVOICED, CANCELLED

**Frontend Pages:** Overview (`/sales`), Orders (`/sales/orders`, `/sales/orders/new`, `/sales/orders/:id`, `/sales/orders/:id/edit`), Customers (`/sales/customers`, `/sales/customers/:id`)

---

#### 4.2.7 Purchasing Module

**Purpose:** Purchase order management, supplier management.

| Endpoint                               | Method | Description           |
| -------------------------------------- | ------ | --------------------- |
| `/api/v1/purchase-orders`              | GET    | List purchase orders  |
| `/api/v1/purchase-orders/{id}`         | GET    | Get purchase order    |
| `/api/v1/purchase-orders`              | POST   | Create purchase order |
| `/api/v1/purchase-orders/{id}/receive` | PUT    | Record goods receipt  |
| `/api/v1/purchase-orders/{id}/cancel`  | PUT    | Cancel purchase order |
| `/api/v1/suppliers`                    | GET    | List suppliers        |
| `/api/v1/suppliers/{id}`               | GET    | Get supplier          |
| `/api/v1/suppliers`                    | POST   | Create supplier       |
| `/api/v1/suppliers/{id}`               | PUT    | Update supplier       |
| `/api/v1/suppliers/{id}`               | DELETE | Soft-delete supplier  |

**Entities:** PurchaseOrder, PurchaseOrderLine, Supplier

**PO Statuses:** DRAFT, SENT, RECEIVED, PARTIAL, CANCELLED

**Frontend Pages:** Overview (`/purchasing`), Orders (`/purchasing/orders`, `/purchasing/orders/new`, `/purchasing/orders/:id`, `/purchasing/orders/:id/edit`), Suppliers (`/purchasing/suppliers`, `/purchasing/suppliers/:id`)

---

#### 4.2.8 Finance Module

**Purpose:** Invoicing, payments, chart of accounts, journal entries, financial reports.

| Endpoint                               | Method | Description            |
| -------------------------------------- | ------ | ---------------------- |
| `/api/v1/invoices`                     | GET    | List invoices          |
| `/api/v1/invoices/{id}`                | GET    | Get invoice            |
| `/api/v1/invoices/{id}/pdf`            | GET    | Download invoice PDF   |
| `/api/v1/invoices`                     | POST   | Create invoice         |
| `/api/v1/invoices/{id}/payments`       | POST   | Record payment         |
| `/api/v1/invoices/{id}/send`           | PUT    | Mark invoice sent      |
| `/api/v1/invoices/{id}/cancel`         | PUT    | Cancel invoice         |
| `/api/v1/accounts`                     | GET    | List chart of accounts |
| `/api/v1/accounts/{id}`                | GET    | Get account            |
| `/api/v1/accounts`                     | POST   | Create account         |
| `/api/v1/accounts/{id}`                | PUT    | Update account         |
| `/api/v1/accounts/{id}`                | DELETE | Delete account         |
| `/api/v1/journal-entries`              | GET    | List journal entries   |
| `/api/v1/journal-entries/{id}`         | GET    | Get journal entry      |
| `/api/v1/journal-entries`              | POST   | Create journal entry   |
| `/api/v1/journal-entries/{id}/post`    | POST   | Post journal entry     |
| `/api/v1/journal-entries/{id}/reverse` | PUT    | Reverse journal entry  |

**Entities:** Account, JournalEntry, JournalEntryLine, Invoice, Payment

**Account Types:** ASSET, LIABILITY, EQUITY, INCOME, EXPENSE
**Invoice Statuses:** DRAFT, SENT, PAID, OVERDUE, CANCELLED
**Payment Methods:** CASH, CARD, BANK_TRANSFER, CHEQUE

**Frontend Pages:** Overview (`/finance`), Invoices (`/finance/invoices`, `/finance/invoices/new`, `/finance/invoices/:id`), Accounts (`/finance/accounts`), Journal (`/finance/journal`, `/finance/journal/new`), Reports (`/finance/reports`)

---

#### 4.2.9 Project Management Module (✅ Done)

**Purpose:** Manage projects, tasks, and timelines.

| Endpoint                       | Method | Description                                           |
| ------------------------------ | ------ | ----------------------------------------------------- |
| `/api/v1/projects`             | GET    | List projects (paginated, filterable by state/search) |
| `/api/v1/projects/{id}`        | GET    | Get project                                           |
| `/api/v1/projects`             | POST   | Create project                                        |
| `/api/v1/projects/{id}/state`  | PATCH  | Update project state                                  |
| `/api/v1/projects/{id}`        | DELETE | Delete project                                        |
| `/api/v1/projects/{id}/stages` | GET    | List task stages for project                          |
| `/api/v1/projects/{id}/tasks`  | GET    | List tasks for project                                |
| `/api/v1/projects/{id}/tasks`  | POST   | Create task in project                                |
| `/api/v1/projects/{id}/gantt`  | GET    | Gantt chart data                                      |
| `/api/v1/tasks/{id}`           | PUT    | Update task                                           |

**Entities:** Project, Task, TaskStage

**Project States:** PLANNING, ACTIVE, ON_HOLD, COMPLETED, CANCELLED
**Default Stages (auto-created):** To Do, In Progress, Review, Done

**Relationships:**

- Project → Customer (FK: customer_id)
- Task → Project (FK: project_id)
- Task → Employee (FK: assigned_to)
- Task → TaskStage (FK: stage_id)
- TaskStage → Project (FK: project_id)

**Frontend Pages (implemented):** Projects List (`/projects`), Project Details (`/projects/:id`), Gantt View (`/projects/:id/gantt`)

---

#### 4.2.10 CRM Module (✅ FE implemented, ❌ BE not started)

**Purpose:** Lead tracking, pipeline management, conversion to customers.

| Endpoint                               | Method | Description              |
| -------------------------------------- | ------ | ------------------------ |
| `/api/v1/crm/dashboard`                | GET    | CRM dashboard stats      |
| `/api/v1/crm/leads`                    | GET    | List leads (filterable)  |
| `/api/v1/crm/leads`                    | POST   | Create lead              |
| `/api/v1/crm/leads/{id}`               | GET    | Get lead                 |
| `/api/v1/crm/leads/{id}`               | PUT    | Update lead              |
| `/api/v1/crm/leads/{id}/convert`       | POST   | Convert lead to customer |
| `/api/v1/crm/pipelines`                | GET    | List pipeline stages     |
| `/api/v1/crm/opportunities/{id}/stage` | PUT    | Update opportunity stage |

**Entities (planned):** Lead, Opportunity, PipelineStage

**Lead Statuses:** NEW, CONTACTED, QUALIFIED, CONVERTED, LOST

**Relationships:**

- Lead → User (assignee)
- Opportunity → Customer, PipelineStage

**Frontend Pages (existing):** CRM Dashboard (`/crm`), Leads List (`/crm/leads`), Lead Details (`/crm/leads/:id`), Pipeline Kanban (`/crm/pipeline`)

> **Note:** CRM pages exist at their routes but have **no sidebar entry** — accessible only via direct URL.

---

#### 4.2.11 Helpdesk Module (✅ Done)

**Purpose:** Ticket management, support workflow.

| Endpoint                                | Method | Description                          |
| --------------------------------------- | ------ | ------------------------------------ |
| `/api/v1/support/tickets`               | GET    | List tickets (paginated, filterable) |
| `/api/v1/support/tickets`               | POST   | Create ticket                        |
| `/api/v1/support/tickets/{id}`          | GET    | Get ticket with comments             |
| `/api/v1/support/tickets/{id}`          | PUT    | Update ticket                        |
| `/api/v1/support/tickets/{id}`          | DELETE | Delete ticket                        |
| `/api/v1/support/tickets/{id}/comments` | POST   | Add comment                          |
| `/api/v1/support/tickets/{id}/close`    | POST   | Close a resolved ticket              |
| `/api/v1/support/tickets/{id}/assign`   | POST   | Assign/reassign ticket               |
| `/api/v1/support/tickets/{id}/stage`    | POST   | Move to different workflow stage     |
| `/api/v1/support/stats`                 | GET    | Helpdesk statistics                  |

**Entities:** Ticket, TicketComment, HelpdeskStage, HelpdeskTeam, HelpdeskCategory, HelpdeskTag, SlaPolicy, KbArticle, TicketAttachment

**Ticket Priorities:** LOW, MEDIUM, HIGH, URGENT
**Ticket Statuses:** OPEN, IN_PROGRESS, RESOLVED, CLOSED
**Ticket Channel:** EMAIL, PHONE, CHAT, PORTAL
**SLA Status:** OK, WARNING, BREACHED

**Relationships:**

- Ticket → Customer (@ManyToOne, FK: customer_id)
- Ticket → Employee (@ManyToOne, FK: assigned_to)
- Ticket → User (@ManyToOne, FK: created_by)
- Ticket → HelpdeskStage (@ManyToOne, FK: stage_id)
- Ticket → HelpdeskTeam (@ManyToOne, FK: team_id)
- Ticket → HelpdeskCategory (@ManyToOne, FK: category_id)
- Ticket → HelpdeskTag (@ManyToMany)
- Ticket → TicketComment (@OneToMany, CASCADE delete)
- TicketComment → User (@ManyToOne, FK: author_id)

**Frontend Pages (implemented):** Tickets List (`/support/tickets`), Ticket Details (`/support/tickets/:id`), Create Ticket (`/support/tickets/new`), Edit Ticket (`/support/tickets/:id/edit`)

---

## 5. API Design

### 5.1 Base URL

```
/api/v1
```

### 5.2 Conventions

| Convention        | Value                                                         |
| ----------------- | ------------------------------------------------------------- |
| Versioning        | URL-based (`/v1`)                                             |
| Pagination        | `?page=0&size=20` (0-indexed)                                 |
| Date Format       | ISO 8601 (`2026-03-26T10:30:00Z`)                             |
| ID Format         | Auto-increment Long                                           |
| Response Envelope | `ApiResponse<T>` with `success`, `data`, `error`, `timestamp` |
| Error Format      | `{ "code": "ERR_CODE", "message": "..." }`                    |

### 5.3 Standard Response Envelope

```json
// Success
{ "success": true, "data": { ... }, "timestamp": "2026-..." }

// Paginated
{ "success": true, "data": { "content": [...], "totalElements": 100, "totalPages": 5, "size": 20, "number": 0, "first": true, "last": false } }

// Error
{ "success": false, "error": { "code": "NOT_FOUND", "message": "..." }, "timestamp": "2026-..." }
```

### 5.4 HTTP Status Codes

| Code | Usage                          |
| ---- | ------------------------------ |
| 200  | Success                        |
| 201  | Created                        |
| 204  | No Content (delete)            |
| 400  | Bad Request / Validation Error |
| 401  | Unauthorized                   |
| 403  | Forbidden                      |
| 404  | Not Found                      |
| 409  | Conflict                       |
| 422  | Validation Error               |
| 500  | Internal Server Error          |

### 5.5 Common Query Parameters

| Parameter | Type   | Description                         |
| --------- | ------ | ----------------------------------- |
| `page`    | int    | Page number (default: 0)            |
| `size`    | int    | Page size (default: 20, max: 100)   |
| `sort`    | string | Sort field (e.g., `createdAt,desc`) |
| `search`  | string | Search query                        |
| `filter`  | string | Filter expression                   |

### 5.6 Authentication

- JWT-based with access token (15 min) + refresh token (7 days)
- Access token sent as `Authorization: Bearer <token>`
- Refresh token sent in body to `/auth/refresh`
- Password hashing: BCrypt
- Role-based authorization via `@PreAuthorize` or custom filter

---

## 6. Database Design

### 6.1 Migrations (Flyway)

| Version | Name                          | Content                                                                                                                                                                                                        |
| ------- | ----------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| V1      | admin_schema                  | users, roles, audit_logs, settings                                                                                                                                                                             |
| V2      | hr_schema                     | employees, attendance, leave_requests, leave_balances                                                                                                                                                          |
| V3      | inventory                     | products, categories, stock_movements                                                                                                                                                                          |
| V4      | purchasing                    | suppliers, purchase_orders, purchase_order_lines                                                                                                                                                               |
| V5      | sales                         | customers, sales_orders, sales_order_lines                                                                                                                                                                     |
| V6      | finance                       | accounts, journal_entries, journal_entry_lines, invoices, payments                                                                                                                                             |
| V7      | seed_data                     | default roles, admin user, settings                                                                                                                                                                            |
| V8      | test_users                    | test user accounts                                                                                                                                                                                             |
| V9      | fix_attendance_schema         | attendance table fixes                                                                                                                                                                                         |
| V10     | fix_attendance_entity_match   | attendance column alignment                                                                                                                                                                                    |
| V11     | fix_leave_column_names        | leave column fixes                                                                                                                                                                                             |
| V12     | fix_leave_columns             | more leave column fixes                                                                                                                                                                                        |
| V13     | fix_leave_balances_columns    | leave balances column fixes                                                                                                                                                                                    |
| V14     | seed_categories               | default product categories                                                                                                                                                                                     |
| V15     | seed_customers                | default customers                                                                                                                                                                                              |
| V16     | fix_product_column_names      | rename current_stock→stock_quantity, reorder_level→reorder_point                                                                                                                                               |
| V17     | (planned) CRM                 | leads, opportunities, pipeline_stages                                                                                                                                                                          |
| V18     | Helpdesk                      | tickets, ticket_comments                                                                                                                                                                                       |
| V25     | Helpdesk Overhaul             | helpdesk_teams, helpdesk_team_members, helpdesk_stages, helpdesk_categories, helpdesk_tags, helpdesk_ticket_tags, helpdesk_sla_policies, helpdesk_kb_articles, helpdesk_attachments; enhanced helpdesk_tickets |
| V19     | project_schema                | projects, tasks, task_stages                                                                                                                                                                                   |
| V20     | fix_suppliers_purchase_orders | supplier code/tax_id, PO delivery_date                                                                                                                                                                         |

### 6.2 Entity Relationships (Summary)

```
users ──┐
         ├── role_id → roles
         ├── employee_id → employees
roles ── has permissions (JSON)

employees ── has attendance, leave_requests, leave_balances
customers ── has sales_orders, linked to projects
suppliers ── has purchase_orders
products ── belongs to category, has stock_movements
sales_orders ── has lines → products
purchase_orders ── has lines → products
invoices ── belongs to customer, has payments, linked to sales_order
accounts ── chart of accounts (parent/child)
journal_entries ── has lines → accounts
projects ── has tasks, task_stages; linked to customer
tasks ── belongs to project+stage, assigned to employee

audit_logs ── polymorphic (entity_type + entity_id)
```

### 6.3 Enum Reference

| Enum              | Values                                          |
| ----------------- | ----------------------------------------------- |
| ProjectState      | PLANNING, ACTIVE, ON_HOLD, COMPLETED, CANCELLED |
| OrderStatus       | DRAFT, CONFIRMED, SHIPPED, INVOICED, CANCELLED  |
| InvoiceStatus     | DRAFT, SENT, PAID, OVERDUE, CANCELLED           |
| POStatus          | DRAFT, SENT, RECEIVED, PARTIAL, CANCELLED       |
| AccountType       | ASSET, LIABILITY, EQUITY, INCOME, EXPENSE       |
| EmployeeStatus    | ACTIVE, INACTIVE, TERMINATED                    |
| LeaveType         | ANNUAL, SICK, PERSONAL, UNPAID                  |
| LeaveStatus       | PENDING, APPROVED, REJECTED, CANCELLED          |
| AttendanceStatus  | PRESENT, ABSENT, LATE, LEAVE                    |
| StockMovementType | IN, OUT, ADJUSTMENT                             |
| PaymentMethod     | CASH, CARD, BANK_TRANSFER, CHEQUE               |
| LeadStatus        | NEW, CONTACTED, QUALIFIED, CONVERTED, LOST      |
| TicketPriority    | LOW, MEDIUM, HIGH, CRITICAL                     |
| TicketStatus      | OPEN, IN_PROGRESS, RESOLVED, CLOSED             |

---

## 7. Frontend Architecture

### 7.1 Route Structure

```
/login                          Auth
/dashboard                      Dashboard
/profile                        Profile

/inventory/products             Product List
/inventory/products/new         Add Product
/inventory/products/:id         Product Details
/inventory/products/:id/edit    Edit Product
/inventory/categories           Categories

/sales/orders                   Sales Orders
/sales/orders/new               Create Order
/sales/orders/:id               Order Details
/sales/orders/:id/edit          Edit Order
/sales/customers                Customers
/sales/customers/:id            Customer Details

/purchasing/orders              Purchase Orders
/purchasing/orders/new          Create PO
/purchasing/orders/:id          PO Details
/purchasing/orders/:id/edit     Edit PO
/purchasing/suppliers           Suppliers
/purchasing/suppliers/:id       Supplier Details

/finance/invoices               Invoices
/finance/invoices/new           Create Invoice
/finance/invoices/:id           Invoice Details
/finance/accounts               Chart of Accounts
/finance/journal                Journal Entries

/hr/employees                   Employees
/hr/employees/:id               Employee Details
/hr/attendance                  Attendance
/hr/leave                       Leave Requests

/admin/users                    User Management
/admin/settings                 Company Settings
/admin/audit-logs               Audit Logs

/crm                            CRM Dashboard
/crm/leads                      Leads List
/crm/leads/:id                  Lead Details
/crm/pipeline                   Pipeline Kanban

/projects                       Projects List
/projects/:id                   Project Details
/projects/:id/gantt             Gantt View

/support/tickets                Tickets List
/support/tickets/:id            Ticket Details
/support/tickets/new            Create Ticket
/support/tickets/:id/edit       Edit Ticket
```

### 7.2 Navigation Structure

Sidebar organization:

1. Dashboard
2. Projects
3. Inventory (Products, Categories)
4. Sales (Orders, Customers)
5. Purchasing (Suppliers, Orders)
6. Finance (Invoices, Journal, Accounts)
7. Support (Tickets)
8. HR (Employees, Attendance, Leave)
9. Admin (Users, Settings, Audit Logs)
10. Profile + Logout (footer)

### 7.3 Common Components

| Component       | Usage                                                                 |
| --------------- | --------------------------------------------------------------------- |
| DataTable       | All list pages (sorting, filtering, pagination, column customization) |
| FormField       | Form inputs with validation                                           |
| StatusBadge     | Color-coded status indicators                                         |
| PageHeader      | Page titles + breadcrumbs + actions                                   |
| StatCard        | Dashboard metric cards                                                |
| ConfirmModal    | Delete/action confirmation dialogs                                    |
| FileUpload      | Product images, attachments                                           |
| DateRangePicker | Filters and reports date ranges                                       |
| CurrencyInput   | Monetary value inputs                                                 |

### 7.4 State Management

- **AuthContext**: User session, JWT tokens, login/logout
- **ThemeContext**: Light/dark mode preference
- **Local component state**: For page-specific data via `useState`/`useEffect`
- **Custom hooks**: `useCRM`, `useAuth`, etc. for reusable logic

---

## 8. Security

### 8.1 Authentication

- JWT access token: 15-minute expiry
- JWT refresh token: 7-day expiry, stored in Redis for invalidation
- BCrypt password hashing (strength: 10+)
- Login rate limiting (planned)

### 8.2 Authorization

- Role-based access control (RBAC)
- Roles: ADMIN, USER (extensible via Role entity with JSON permissions)
- Endpoint-level security via `@PreAuthorize` or security filter chain
- Frontend route protection via `ProtectedRoute` wrapper

### 8.3 Data Protection

- Input validation on all endpoints (Hibernate Validator)
- SQL injection prevention (parameterized JPA queries)
- XSS prevention (React auto-escaping)
- Audit logging for all CUD operations (`@Auditable` annotation)
- No sensitive data in logs

---

## 9. Infrastructure

### 9.1 Development Environment

| Service             | Technology    | Port |
| ------------------- | ------------- | ---- |
| API Server          | Spring Boot   | 8080 |
| Database            | PostgreSQL 15 | 5432 |
| Cache               | Redis 7       | 6379 |
| Frontend Dev Server | Vite          | 5173 |

### 9.2 Docker Setup

```yaml
# docker-compose.yml
services:
  postgres:
    image: postgres:15
    ports: ["5432:5432"]
    environment:
      POSTGRES_DB: erp
      POSTGRES_USER: erp
      POSTGRES_PASSWORD: erp
  redis:
    image: redis:7-alpine
    ports: ["6379:6379"]
```

### 9.3 Nix Flake

Provides reproducible dev environment with: JDK 21, Maven 3.9+, Git

---

## 10. Testing

### 10.1 Backend Testing

- Framework: JUnit 5 + Mockito
- Tests: 17 test classes, 104 test methods
- Coverage: Controller, Service, Repository layers
- Test profile: `application-test.yml` with in-memory DB or test PostgreSQL
- Key tests: Auth flow, CRUD operations, business logic, error scenarios, authorization

### 10.2 Frontend Testing

- Framework: Vitest + React Testing Library
- Component rendering tests
- Service mocking for API calls
- Form validation tests

### 10.3 Testing Strategy

| Layer      | Type        | What to test                                 |
| ---------- | ----------- | -------------------------------------------- |
| Controller | Unit        | Request mapping, validation, response codes  |
| Service    | Unit        | Business logic, edge cases, transactions     |
| Repository | Integration | Custom queries, pagination, filtering        |
| Component  | Unit        | Rendering, user interactions, state changes  |
| E2E        | Manual      | Full workflows (create → process → complete) |

---

## 11. Development Phases

| Phase | Focus                     | Modules                                                     | Duration |
| ----- | ------------------------- | ----------------------------------------------------------- | -------- |
| 1     | UML Diagrams              | All modules (class, usecase, sequence, component, activity) | 2 weeks  |
| 2     | Shared Diagrams           | ERD, system overview, master usecase                        | 1 week   |
| 3     | Implementation Foundation | Inventory, Sales, Purchasing, Finance (BE + FE)             | 4 weeks  |
| 4     | Development Integration   | HR, Admin, Auth, Dashboard (BE + FE)                        | 4 weeks  |
| 5     | Module Expansion          | CRM, Helpdesk, Project Management                           | 3 weeks  |
| 6     | Polish                    | Testing, optimization, bug fixes, documentation             | 2 weeks  |

---

## 12. Team Structure

### Backend Team

| Name                     | ID        |
| ------------------------ | --------- |
| Ahmed Hany Abo-ElSoud    | 231001623 |
| Omar Hisham Desouky      | 231000256 |
| Abd-ElRahman Yasser Zaky | 23100102  |

### Frontend Team

| Name                       | ID        |
| -------------------------- | --------- |
| Ahmed Mohamed Abd-ElMaboud | 231000224 |
| Mazen Ahmed Refai          | 231001075 |
| Yahia Hany Gaber           | 231000412 |

---

## 13. Development Guidelines

### 13.1 Git Workflow

- Branch from `main`: `feature/<name>`, `fix/<issue>`, `hotfix/<issue>`
- Commits: `type(scope): description` (conventional commits)
- PRs: Squash merge, leader merges only
- Review turnaround: within 24 hours

### 13.2 Backend Conventions

- Package per module: `com.erp.<module>.*`
- Layers: controller → service → repository → entity
- Lombok: `@Getter @Setter @NoArgsConstructor @AllArgsConstructor @Builder`
- DTOs for all API contracts (never expose entities)
- Soft delete for business entities
- `@Auditable` for audit logging on CUD operations

### 13.3 Frontend Conventions

- Functional components with hooks
- Explicit TypeScript interfaces (no `any`)
- One component per file (max ~150 lines)
- Barrel exports via `index.ts`
- CSS Modules or Ant Design theming

---

## 14. Current Status & Known Gaps

| Area                  | Status                         | Action Needed                                           |
| --------------------- | ------------------------------ | ------------------------------------------------------- |
| 7 Core BE modules     | ✅ Done                        | -                                                       |
| Project Management BE | ✅ Done                        | -                                                       |
| CRM BE                | ❌ Missing                     | Implement V17 migration + entities/services/controllers |
| Helpdesk BE           | ✅ Done                        | V22+V25 migrations + entities + services + controllers  |
| CRM FE                | ✅ Done (no sidebar)           | Add CRM to sidebar navigation                           |
| Project Management FE | ✅ Done                        | -                                                       |
| Helpdesk FE           | ✅ Done                        | -                                                       |
| Flyway V10 checksum   | ⚠️ Issue #42                   | `flyway repair` if using old DB                         |
| Docker daemon         | ⚠️ Not accessible without sudo | Use podman as alternative                               |
| Frontend tests        | ❌ Missing                     | Setup Vitest + write tests                              |
| E2E tests             | ❌ Missing                     | Plan and implement                                      |
| CI/CD pipeline        | ❌ Missing                     | Setup GitHub Actions                                    |
| Deployment docs       | ❌ Missing                     | Docker compose for production                           |
