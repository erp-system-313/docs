# Deliverables 02 - Shared System Diagrams

## Overview

These are cross-cutting diagrams that require collaboration across all pairs and represent system-wide views.

---

## Diagram 1: Master Use Case

| Field | Value |
|-------|-------|
| **File** | `diagrams/usecase/system-overview.md` |
| **Type** | Use Case |
| **Owner** | Pair C (leads) |
| **Contributors** | All pairs add their actors/use cases |

### Purpose
Combine all module use cases into a single system-wide use case diagram showing actors and their interactions with the system.

### Contents
- **Actors:** Guest, Staff, Manager, Admin
- **Use Cases by Module:**
  - Authentication: Login, Logout, Reset Password
  - Dashboard: View Dashboard
  - Inventory: Manage Products, Manage Categories, View Stock
  - Sales: Manage Orders, Manage Customers, Manage Invoices
  - Purchasing: Manage Purchase Orders, Manage Suppliers
  - Finance: View Accounts, Manage Journal, Generate Reports
  - HR: Manage Employees, Track Attendance, Request Leave
  - Admin: Manage Users, Configure Settings, View Audit Logs
  - Support: Create Ticket

---

## Diagram 2: Entity Relationship (ERD)

| Field | Value |
|-------|-------|
| **File** | `diagrams/class/entity-relationship.md` |
| **Type** | Class (ERD) |
| **Owner** | BE1 (leads) |
| **Contributors** | All backend devs define their entities |

### Purpose
Show all database entities and their relationships for the entire ERP system.

### Entities to Include

| Entity | Attributes |
|--------|------------|
| **User** | id, email, password_hash, role_id, employee_id, is_active, created_at, updated_at |
| **Role** | id, name, permissions (JSON) |
| **Employee** | id, user_id, first_name, last_name, email, phone, hire_date, salary, department, position |
| **Product** | id, sku, name, category_id, unit_price, cost_price, reorder_level, image_url, is_active |
| **Category** | id, name, description, parent_id |
| **Supplier** | id, name, contact_person, email, phone, address, is_active |
| **StockMovement** | id, product_id, type (IN/OUT), quantity, reference_type, reference_id, date, notes |
| **Customer** | id, name, email, phone, address, credit_limit, payment_terms, is_active |
| **SalesOrder** | id, order_number, customer_id, date, status, total_amount, notes |
| **SalesOrderLine** | id, order_id, product_id, quantity, unit_price, line_total |
| **Invoice** | id, invoice_number, sales_order_id, date, due_date, status, total, tax_amount |
| **Payment** | id, invoice_id, amount, date, method, reference, notes |
| **PurchaseOrder** | id, po_number, supplier_id, date, status, total_amount, expected_date |
| **PurchaseOrderLine** | id, order_id, product_id, quantity, unit_price, line_total, received_qty |
| **Account** | id, code, name, type (ASSET/LIABILITY/EQUITY/INCOME/EXPENSE), parent_id, balance |
| **JournalEntry** | id, date, description, reference, status, created_by, created_at |
| **JournalEntryLine** | id, entry_id, account_id, debit, credit |
| **Attendance** | id, employee_id, date, check_in, check_out, status, notes |
| **LeaveRequest** | id, employee_id, start_date, end_date, type, status, reason, approved_by, approved_at |

### Relationships
- User 1:1 Employee
- User N:1 Role
- Product N:1 Category
- Product N:1 Supplier
- SalesOrder N:1 Customer
- SalesOrderLine N:1 SalesOrder
- SalesOrderLine N:1 Product
- Invoice N:1 SalesOrder
- Payment N:1 Invoice
- PurchaseOrder N:1 Supplier
- Attendance N:1 Employee
- LeaveRequest N:1 Employee

---

## Diagram 3: System Sequence

| Field | Value |
|-------|-------|
| **File** | `diagrams/sequence/system-flows.md` |
| **Type** | Sequence |
| **Owner** | Pair B (leads) |
| **Contributors** | Cross-module flows documented |

### Purpose
Show cross-module system flows and how different components interact across the entire ERP system.

### Flows to Include

**Flow 1: Sales Order to Invoice to Payment**
1. Customer selects products
2. System validates stock availability
3. Staff creates Sales Order
4. System deducts stock (Inventory)
5. Sales Order confirmed
6. Invoice generated automatically
7. Customer makes payment
8. System records payment
9. Invoice marked as paid

**Flow 2: Purchase Order Receiving**
1. Staff creates Purchase Order
2. Supplier receives PO
3. Goods received
4. Staff records receipt
5. System updates stock levels (Inventory)
6. Invoice received from supplier
7. Payment processed (Finance)

**Flow 3: Employee Leave Request**
1. Employee submits leave request
2. Manager receives notification
3. Manager approves/rejects
4. System updates leave balance
5. Employee notified of decision

**Flow 4: User Authentication**
1. User enters credentials
2. System validates credentials
3. JWT token generated
4. Token returned to client
5. Subsequent requests include token
6. Token validated on each request

---

## Diagram 4: Activity Workflows

| Field | Value |
|-------|-------|
| **File** | `diagrams/activity/workflows.md` |
| **Type** | Activity |
| **Owner** | Pair A (leads) |
| **Contributors** | Order fulfillment, leave approval workflows |

### Purpose
Show business workflows and processes using activity diagrams.

### Activities to Include

**Activity 1: Order Fulfillment**
```
Start → Receive Order → Validate Stock → [Stock OK?] → Create Invoice → Ship Order → Notify Customer → End
                      ↓                       
                 [Stock Low?] → Alert Inventory → End
```

**Activity 2: Leave Approval**
```
Start → Employee Submit → Manager Review → [Approved?] → Update Leave Balance → Notify Employee → End
                                ↓
                          [Rejected?] → Notify Employee with Reason → End
```

**Activity 3: Purchase Requisition**
```
Start → Create PR → Manager Approval → [Approved?] → Create PO → Send to Supplier → End
                         ↓
                   [Rejected?] → Notify Requester → End
```

**Activity 4: Invoice Payment**
```
Start → Receive Payment → Validate Amount → [Valid?] → Update Invoice → Record Transaction → End
                       ↓
                 [Invalid?] → Reject Payment → End
```

**Activity 5: User Onboarding**
```
Start → Admin Creates User → Assign Role → Send Welcome Email → User Sets Password → Account Active → End
```

---

## Status

| Diagram | Status |
|---------|--------|
| Master Use Case | ❌ Pending |
| ERD | ❌ Pending |
| System Sequence | ❌ Pending |
| Activity Workflows | ❌ Pending |

---

## Previous Deliverables

See [01-deliverables.md](./01-deliverables.md) for completed diagrams.
