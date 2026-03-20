# Navigation Structure

## Sidebar Navigation

The sidebar contains the following menu structure:

| Menu Item | Route | Type |
|-----------|-------|------|
| Dashboard | `/dashboard` | Link |
| **Inventory** | Collapsible group | |
| - Overview | `/inventory` | Link |
| - Products | `/inventory/products` | Link |
| - Categories | `/inventory/categories` | Link |
| **Sales** | Collapsible group | |
| - Overview | `/sales` | Link |
| - Orders | `/sales/orders` | Link |
| - Customers | `/sales/customers` | Link |
| **Purchasing** | Collapsible group | |
| - Overview | `/purchasing` | Link |
| - Orders | `/purchasing/orders` | Link |
| - Suppliers | `/purchasing/suppliers` | Link |
| **Finance** | Collapsible group | |
| - Overview | `/finance` | Link |
| - Invoices | `/finance/invoices` | Link |
| - Accounts | `/finance/accounts` | Link |
| - Journal | `/finance/journal` | Link |
| - Reports | `/finance/reports` | Link |
| **HR** | Collapsible group | |
| - Overview | `/hr` | Link |
| - Employees | `/hr/employees` | Link |
| - Attendance | `/hr/attendance` | Link |
| - Leave | `/hr/leave` | Link |
| **Admin** | Collapsible group | |
| - Overview | `/admin` | Link |
| - Users | `/admin/users` | Link |
| - Settings | `/admin/settings` | Link |
| - Audit Logs | `/admin/audit-logs` | Link |
| Support | `/support` | Link |
| Profile | `/profile` | Link + Logout button |

## Navigation Principles

### 1. Sidebar Menu Structure
- **Modules** are collapsible menu groups (prefixed with ▼)
- **Overview pages** exist as parent menu items
- **Sub-pages** are indented child items
- **Profile and Logout** are fixed at the bottom

### 2. Breadcrumbs
Required on all detail and edit pages. Format:
```
Module > SubModule > Page Title
```
Examples:
- `Sales > Orders > ORD-001`
- `Inventory > Products > SKU-1234`
- `HR > Employees > John Doe`

### 3. Tabs
Detail pages with multiple sub-sections use tabs:
- Product Details: Basic Info | Pricing | Stock History | Suppliers
- Customer Details: Profile | Contact | Order History | Payment Terms
- Employee Details: Personal Info | Job Details | Documents | Attendance

### 4. Module Overview Pages
Each module has an overview page that serves as:
- The default view when entering the module
- A dashboard with module-specific metrics and quick actions

| Module | Route | Overview Content |
|--------|-------|------------------|
| Dashboard | `/dashboard` | Key metrics, charts, recent activity |
| Inventory | `/inventory` | Stock levels, movements, alerts |
| Sales | `/sales` | Sales summary, pending orders, top customers |
| Purchasing | `/purchasing` | PO status, supplier performance |
| Finance | `/finance` | Revenue, expenses, cash flow summary |
| HR | `/hr` | Headcount, attendance rates, pending leaves |
| Admin | `/admin` | System health, user activity |

---

## Route Index

### Dashboard & Common
| Page | Route |
|------|-------|
| Dashboard | `/dashboard` |
| Profile | `/profile` |

### Inventory Management
| Page | Route |
|------|-------|
| Overview | `/inventory` |
| Product List | `/inventory/products` |
| Product Details | `/inventory/products/:id` |
| Product Edit | `/inventory/products/:id/edit` |
| Add Product | `/inventory/products/new` |
| Categories | `/inventory/categories` |

### Sales & Customers
| Page | Route |
|------|-------|
| Overview | `/sales` |
| Sales Orders | `/sales/orders` |
| Create Order | `/sales/orders/new` |
| Order Details | `/sales/orders/:id` |
| Edit Order | `/sales/orders/:id/edit` |
| Customers List | `/sales/customers` |
| Customer Details | `/sales/customers/:id` |

### Purchasing & Suppliers
| Page | Route |
|------|-------|
| Overview | `/purchasing` |
| Purchase Orders | `/purchasing/orders` |
| Create PO | `/purchasing/orders/new` |
| PO Details | `/purchasing/orders/:id` |
| Edit PO | `/purchasing/orders/:id/edit` |
| Suppliers List | `/purchasing/suppliers` |
| Supplier Details | `/purchasing/suppliers/:id` |

### Finance & Accounting
| Page | Route |
|------|-------|
| Overview | `/finance` |
| Invoices List | `/finance/invoices` |
| Invoice Details | `/finance/invoices/:id` |
| Create Invoice | `/finance/invoices/new` |
| Chart of Accounts | `/finance/accounts` |
| Journal Entries | `/finance/journal` |
| Financial Reports | `/finance/reports` |

### Human Resources
| Page | Route |
|------|-------|
| Overview | `/hr` |
| Employees List | `/hr/employees` |
| Employee Details | `/hr/employees/:id` |
| Attendance | `/hr/attendance` |
| Leave Requests | `/hr/leave` |

### Administration
| Page | Route |
|------|-------|
| Overview | `/admin` |
| User Management | `/admin/users` |
| Company Settings | `/admin/settings` |
| Audit Logs | `/admin/audit-logs` |

### Support
| Page | Route |
|------|-------|
| Support/Tickets | `/support` |

---

## URL Naming Conventions

- **Lowercase** with hyphens (kebab-case)
- **Plural nouns** for list pages (products, orders, customers)
- **No verbs** in routes (actions are determined by HTTP method or page type)
- **ID parameters** use colon prefix (`:id`, `:orderId`)

## Page Types

| Type | Suffix | Example |
|------|--------|---------|
| List | (none) | `/inventory/products` |
| Detail | (none) | `/inventory/products/:id` |
| Create | `/new` | `/inventory/products/new` |
| Edit | `/edit` | `/inventory/products/:id/edit` |
| Settings | `/settings` | `/admin/settings` |
