# ERP System Pages Architecture

> **⚠️ This document is partially outdated.** See the comprehensive [requirements.html](../requirements.html) for the current route map (55+ pages across 12 modules).

## Overview
This document outlines all pages/routes for the ERP system webapp, organized by module.

---

## Module A: Dashboard & Common (2 pages)

| # | Page | Route | Description |
|---|------|-------|-------------|
| 1 | Dashboard | `/dashboard` | Overview with key metrics, charts, widgets (total sales, low stock alerts, pending invoices, recent orders) |
| 2 | User Profile | `/profile` | View/edit personal info, change password, notification preferences |

---

## Module B: Inventory Management (6 pages)

| # | Page | Route | Description |
|---|------|-------|-------------|
| 3 | Inventory Dashboard | `/inventory` | Stock levels, movements, reorder points |
| 4 | Product List | `/inventory/products` | Searchable table with filters (category, supplier), actions: edit, delete, view details |
| 5 | Product Details | `/inventory/products/:id` | Detailed view with tabs: basic info, pricing, stock history, suppliers |
| 6 | Product Edit | `/inventory/products/:id/edit` | Edit product form |
| 7 | Add New Product | `/inventory/products/new` | Form: name, SKU, category, unit, price, reorder level, image upload |
| 8 | Categories | `/inventory/categories` | CRUD for product categories (list, add, edit, delete) |

---

## Module C: Sales & Customers (5 pages)

| # | Page | Route | Description |
|---|------|-------|-------------|
| 9 | Sales Orders | `/sales/orders` | List with status filters (draft, confirmed, shipped, invoiced), date filters |
| 10 | Create Sales Order | `/sales/orders/new` | Multi-step: select customer, add products, apply discounts, calculate totals |
| 11 | Edit Sales Order | `/sales/orders/:id/edit` | Modify existing sales order |
| 12 | Customers List | `/sales/customers` | Searchable table with contacts, outstanding balance |
| 13 | Customer Details | `/sales/customers/:id` | Profile, contact info, order history, payment terms |

---

## Module D: Purchasing & Suppliers (4 pages)

| # | Page | Route | Description |
|---|------|-------|-------------|
| 14 | Purchase Orders | `/purchasing/orders` | List of POs to suppliers with status |
| 15 | Create Purchase Order | `/purchasing/orders/new` | Select supplier, add products, quantities, pricing |
| 16 | Suppliers List | `/purchasing/suppliers` | Manage supplier details |
| 17 | Supplier Details | `/purchasing/suppliers/:id` | Profile, contact info, purchase history, payment terms |

---

## Module E: Finance & Accounting (6 pages)

| # | Page | Route | Description |
|---|------|-------|-------------|
| 18 | Invoices List | `/finance/invoices` | List of invoices (paid/unpaid/overdue), download PDF, record payment |
| 19 | Invoice Details | `/finance/invoices/:id` | View invoice details, payment history |
| 20 | Create Invoice | `/finance/invoices/new` | Generate invoice from sales order or manual |
| 21 | Chart of Accounts | `/finance/accounts` | List of accounts (assets, liabilities, equity, income, expenses) |
| 22 | Journal Entries | `/finance/journal` | Record manual transactions (debit/credit) |
| 23 | Financial Reports | `/finance/reports` | Income statement, balance sheet, cash flow with date filters |

---

## Module F: Human Resources (4 pages)

| # | Page | Route | Description |
|---|------|-------|-------------|
| 24 | Employees List | `/hr/employees` | Directory of employees with search/filter |
| 25 | Employee Details | `/hr/employees/:id` | Personal info, job details, salary, documents |
| 26 | Attendance | `/hr/attendance` | Clock-in/out, attendance calendar view |
| 27 | Leave Requests | `/hr/leave` | Apply for leave, approval workflow, leave balance |

---

## Module G: Administration & Settings (2 pages)

| # | Page | Route | Description |
|---|------|-------|-------------|
| 28 | User Management | `/admin/users` | CRUD for system users, assign roles (admin, manager, staff) |
| 29 | Company Settings | `/admin/settings` | Configure company name, tax rates, fiscal year, email templates |

---

## Additional Pages

| # | Page | Route | Description |
|---|------|-------|-------------|
| 30 | Audit Logs | `/admin/audit-logs` | Track system changes and user actions |
| 31 | Support/Tickets | `/support` | Help desk ticket system for user issues |

---

## Page Summary

| Module | Page Count |
|--------|------------|
| Dashboard & Common | 2 |
| Inventory Management | 6 |
| Sales & Customers | 5 |
| Purchasing & Suppliers | 4 |
| Finance & Accounting | 6 |
| Human Resources | 4 |
| Administration | 2 |
| Additional | 2 |
| **Total** | **31 pages** |

---

## Route Structure

```
/dashboard
/profile

/inventory
/inventory/products
/inventory/products/new
/inventory/products/:id
/inventory/products/:id/edit
/inventory/categories

/sales
/sales/orders
/sales/orders/new
/sales/orders/:id
/sales/orders/:id/edit
/sales/customers
/sales/customers/:id

/purchasing
/purchasing/orders
/purchasing/orders/new
/purchasing/orders/:id
/purchasing/orders/:id/edit
/purchasing/suppliers
/purchasing/suppliers/:id

/finance
/finance/invoices
/finance/invoices/new
/finance/invoices/:id
/finance/accounts
/finance/journal
/finance/reports

/hr
/hr/employees
/hr/employees/:id
/hr/attendance
/hr/leave

/admin
/admin/users
/admin/settings
/admin/audit-logs

/support
```

---

## Page Component Structure

```
src/
├── pages/
│   ├── common/
│   │   ├── Dashboard.tsx
│   │   └── Profile.tsx
│   ├── inventory/
│   │   ├── InventoryDashboard.tsx
│   │   ├── ProductList.tsx
│   │   ├── ProductDetails.tsx
│   │   ├── ProductForm.tsx (shared for new/edit)
│   │   └── Categories.tsx
│   ├── sales/
│   │   ├── SalesOrders.tsx
│   │   ├── SalesOrderForm.tsx
│   │   ├── CustomersList.tsx
│   │   └── CustomerDetails.tsx
│   ├── purchasing/
│   │   ├── PurchaseOrders.tsx
│   │   ├── PurchaseOrderForm.tsx
│   │   ├── SuppliersList.tsx
│   │   └── SupplierDetails.tsx
│   ├── finance/
│   │   ├── Invoices.tsx
│   │   ├── InvoiceDetails.tsx
│   │   ├── InvoiceForm.tsx
│   │   ├── ChartOfAccounts.tsx
│   │   ├── JournalEntries.tsx
│   │   └── FinancialReports.tsx
│   ├── hr/
│   │   ├── EmployeesList.tsx
│   │   ├── EmployeeDetails.tsx
│   │   ├── Attendance.tsx
│   │   └── LeaveRequests.tsx
│   ├── admin/
│   │   ├── UserManagement.tsx
│   │   ├── CompanySettings.tsx
│   │   └── AuditLogs.tsx
│   └── support/
│       └── Support.tsx
```

---

## Reusable Components Needed

| Component | Usage |
|-----------|-------|
| DataTable | All list pages (sorting, filtering, pagination) |
| FormModal | Quick create/edit in list views |
| StatCard | Dashboard widgets |
| ChartContainer | Dashboard charts |
| ProductSelector | Sales order, purchase order forms |
| CustomerSelector | Sales order form |
| SupplierSelector | Purchase order form |
| LineItemTable | Order forms (dynamic rows) |
| CurrencyInput | All monetary fields |
| DateRangePicker | Filters and reports |
| StatusBadge | Order status, invoice status |
| TabPanel | Detail pages with multiple sections |
| FileUpload | Product images, documents |
| ApprovalWorkflow | Leave requests, purchase orders |
