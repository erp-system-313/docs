# Team Roles & UML Diagram Assignment

## UML Diagram Distribution

### Pair A - Inventory & Purchasing (FE1 + BE1)

| Diagram Type | Deliverable | File Location |
|--------------|-------------|---------------|
| **Use Case** | Product management use cases (add, edit, delete, view stock) | `diagrams/usecase/inventory.md` |
| **Use Case** | Purchase order use cases (create PO, receive goods, cancel) | `diagrams/usecase/purchasing.md` |
| **Sequence** | Stock update sequence (receive inventory, deduct stock) | `diagrams/sequence/stock-updates.md` |
| **Class** | Product, Category, Supplier entity classes | `diagrams/class/inventory.md` |
| **Class** | PurchaseOrder, PurchaseOrderLine, StockMovement | `diagrams/class/purchasing.md` |
| **Component** | ProductList, ProductForm, ProductDetails components | `diagrams/component/product-pages.md` |
| **Component** | PurchaseOrderForm, SupplierList, SupplierDetails | `diagrams/component/purchasing-pages.md` |

---

### Pair B - Sales, Customers & Finance (FE2 + BE2)

| Diagram Type | Deliverable | File Location |
|--------------|-------------|---------------|
| **Use Case** | Sales order use cases (create, confirm, ship, invoice) | `diagrams/usecase/sales.md` |
| **Use Case** | Invoice and payment use cases | `diagrams/usecase/finance.md` |
| **Sequence** | Order-to-invoice-to-payment flow | `diagrams/sequence/sales-flow.md` |
| **Sequence** | Payment recording sequence | `diagrams/sequence/payment.md` |
| **Class** | Customer, SalesOrder, SalesOrderLine | `diagrams/class/sales.md` |
| **Class** | Invoice, Payment, Account, JournalEntry | `diagrams/class/finance.md` |
| **Component** | SalesOrderForm, CustomersList, CustomerDetails | `diagrams/component/sales-pages.md` |
| **Component** | InvoiceList, InvoiceForm, JournalEntries | `diagrams/component/finance-pages.md` |

---

### Pair C - HR, Admin & Dashboard (FE3 + BE3)

| Diagram Type | Deliverable | File Location |
|--------------|-------------|---------------|
| **Use Case** | Authentication & user management use cases | `diagrams/usecase/auth.md` |
| **Use Case** | HR use cases (attendance, leave requests) | `diagrams/usecase/hr.md` |
| **Sequence** | Login/JWT authentication sequence | `diagrams/sequence/auth.md` |
| **Sequence** | Leave approval workflow sequence | `diagrams/sequence/leave-approval.md` |
| **Class** | User, Role, Permission | `diagrams/class/auth.md` |
| **Class** | Employee, Attendance, LeaveRequest | `diagrams/class/hr.md` |
| **Component** | Dashboard widgets, navigation layout | `diagrams/component/dashboard.md` |
| **Component** | UserManagement, CompanySettings, Profile | `diagrams/component/admin-pages.md` |
| **Component** | EmployeeDetails, Attendance, LeaveRequests | `diagrams/component/hr-pages.md` |

---

## Shared Deliverables (All Pairs Contribute)

| Diagram Type | Lead | Contributors | File Location |
|--------------|------|--------------|---------------|
| **Master Use Case** | Pair C | All pairs add their actors/use cases | `diagrams/usecase/system-overview.md` |
| **ERD** | BE1 | All backend devs define their entities | `diagrams/class/entity-relationship.md` |
| **System Sequence** | Pair B | Cross-module flows documented | `diagrams/sequence/system-flows.md` |
| **Activity** | Pair A | Order fulfillment, leave approval workflows | `diagrams/activity/workflows.md` |

---

## Deliverable Summary by Pair

### Pair A (FE1 + BE1)
- 2 Use Case diagrams
- 1 Sequence diagram
- 2 Class diagrams
- 2 Component diagrams
- Contributes to: Master Use Case, ERD, System Sequence, Activity

### Pair B (FE2 + BE2)
- 2 Use Case diagrams
- 2 Sequence diagrams
- 2 Class diagrams
- 2 Component diagrams
- Contributes to: Master Use Case, ERD, System Sequence, Activity

### Pair C (FE3 + BE3)
- 2 Use Case diagrams
- 2 Sequence diagrams
- 2 Class diagrams
- 3 Component diagrams
- Leads: Master Use Case, System Sequence, Activity

---

## File Structure

```
docs/architecture/diagrams/
├── usecase/
│   ├── system-overview.md    (Pair C - master)
│   ├── inventory.md         (Pair A)
│   ├── purchasing.md         (Pair A)
│   ├── sales.md              (Pair B)
│   ├── finance.md            (Pair B)
│   ├── auth.md               (Pair C)
│   └── hr.md                 (Pair C)
├── sequence/
│   ├── system-flows.md      (Pair B - master)
│   ├── stock-updates.md      (Pair A)
│   ├── sales-flow.md         (Pair B)
│   ├── payment.md            (Pair B)
│   ├── auth.md               (Pair C)
│   └── leave-approval.md     (Pair C)
├── class/
│   ├── entity-relationship.md (BE1 - master)
│   ├── inventory.md          (Pair A)
│   ├── purchasing.md          (Pair A)
│   ├── sales.md              (Pair B)
│   ├── finance.md            (Pair B)
│   ├── auth.md               (Pair C)
│   └── hr.md                 (Pair C)
├── component/
│   ├── product-pages.md      (Pair A)
│   ├── purchasing-pages.md    (Pair A)
│   ├── sales-pages.md        (Pair B)
│   ├── finance-pages.md      (Pair B)
│   ├── dashboard.md           (Pair C)
│   ├── admin-pages.md        (Pair C)
│   └── hr-pages.md           (Pair C)
└── activity/
    └── workflows.md          (Pair A - lead)
```
