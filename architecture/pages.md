# ERP System Pages Architecture

## Overview

This document outlines all pages/routes for the ERP system webapp, organized by module.

---

## Module A: Dashboard & Common (2 pages)

| #   | Page         | Route        | Description                                                                                                 |
| --- | ------------ | ------------ | ----------------------------------------------------------------------------------------------------------- |
| 1   | Dashboard    | `/dashboard` | Overview with key metrics, charts, widgets (total sales, low stock alerts, pending invoices, recent orders) |
| 2   | User Profile | `/profile`   | View/edit personal info, change password, notification preferences                                          |

---

## Module B: Inventory Management (6 pages)

| #   | Page                | Route                          | Description                                                                             |
| --- | ------------------- | ------------------------------ | --------------------------------------------------------------------------------------- |
| 3   | Inventory Dashboard | `/inventory`                   | Stock levels, movements, reorder points                                                 |
| 4   | Product List        | `/inventory/products`          | Searchable table with filters (category, supplier), actions: edit, delete, view details |
| 5   | Product Details     | `/inventory/products/:id`      | Detailed view with tabs: basic info, pricing, stock history, suppliers                  |
| 6   | Product Edit        | `/inventory/products/:id/edit` | Edit product form                                                                       |
| 7   | Add New Product     | `/inventory/products/new`      | Form: name, SKU, category, unit, price, reorder level, image upload                     |
| 8   | Categories          | `/inventory/categories`        | CRUD for product categories (list, add, edit, delete)                                   |

---

## Module C: Sales & Customers (5 pages)

| #   | Page               | Route                    | Description                                                                  |
| --- | ------------------ | ------------------------ | ---------------------------------------------------------------------------- |
| 9   | Sales Orders       | `/sales/orders`          | List with status filters (draft, confirmed, shipped, invoiced), date filters |
| 10  | Create Sales Order | `/sales/orders/new`      | Multi-step: select customer, add products, apply discounts, calculate totals |
| 11  | Edit Sales Order   | `/sales/orders/:id/edit` | Modify existing sales order                                                  |
| 12  | Customers List     | `/sales/customers`       | Searchable table with contacts, outstanding balance                          |
| 13  | Customer Details   | `/sales/customers/:id`   | Profile, contact info, order history, payment terms                          |

---

## Module D: Purchasing & Suppliers (4 pages)

| #   | Page                  | Route                       | Description                                            |
| --- | --------------------- | --------------------------- | ------------------------------------------------------ |
| 14  | Purchase Orders       | `/purchasing/orders`        | List of POs to suppliers with status                   |
| 15  | Create Purchase Order | `/purchasing/orders/new`    | Select supplier, add products, quantities, pricing     |
| 16  | Suppliers List        | `/purchasing/suppliers`     | Manage supplier details                                |
| 17  | Supplier Details      | `/purchasing/suppliers/:id` | Profile, contact info, purchase history, payment terms |

---

## Module E: Finance & Accounting (6 pages)

| #   | Page              | Route                   | Description                                                          |
| --- | ----------------- | ----------------------- | -------------------------------------------------------------------- |
| 18  | Invoices List     | `/finance/invoices`     | List of invoices (paid/unpaid/overdue), download PDF, record payment |
| 19  | Invoice Details   | `/finance/invoices/:id` | View invoice details, payment history                                |
| 20  | Create Invoice    | `/finance/invoices/new` | Generate invoice from sales order or manual                          |
| 21  | Chart of Accounts | `/finance/accounts`     | List of accounts (assets, liabilities, equity, income, expenses)     |
| 22  | Journal Entries   | `/finance/journal`      | Record manual transactions (debit/credit)                            |
| 23  | Financial Reports | `/finance/reports`      | Income statement, balance sheet, cash flow with date filters         |

---

## Module F: Human Resources (4 pages)

| #   | Page             | Route               | Description                                       |
| --- | ---------------- | ------------------- | ------------------------------------------------- |
| 24  | Employees List   | `/hr/employees`     | Directory of employees with search/filter         |
| 25  | Employee Details | `/hr/employees/:id` | Personal info, job details, salary, documents     |
| 26  | Attendance       | `/hr/attendance`    | Clock-in/out, attendance calendar view            |
| 27  | Leave Requests   | `/hr/leave`         | Apply for leave, approval workflow, leave balance |

---

## Module G: CRM (4 pages)

| #   | Page            | Route            | Description                                                                       |
| --- | --------------- | ---------------- | --------------------------------------------------------------------------------- |
| 30  | CRM Dashboard   | `/crm`           | Pipeline overview, KPIs (total leads, conversion rate, pipeline value)            |
| 31  | Leads List      | `/crm/leads`     | Searchable table with status filters (NEW, CONTACTED, QUALIFIED, CONVERTED, LOST) |
| 32  | Lead Details    | `/crm/leads/:id` | Full lead profile with convert-to-customer action                                 |
| 33  | Pipeline Kanban | `/crm/pipeline`  | Drag-and-drop Kanban board with opportunity cards                                 |

---

## Module H: Project Management (3 pages)

| #   | Page            | Route                 | Description                                                                           |
| --- | --------------- | --------------------- | ------------------------------------------------------------------------------------- |
| 34  | Projects List   | `/projects`           | List of projects with state filters (PLANNING, ACTIVE, ON_HOLD, COMPLETED, CANCELLED) |
| 35  | Project Details | `/projects/:id`       | Project info, tasks list by stage, team members                                       |
| 36  | Gantt View      | `/projects/:id/gantt` | Timeline view of tasks with dependencies                                              |

---

## Module I: Helpdesk / Support (3 pages)

| #   | Page           | Route                  | Description                                          |
| --- | -------------- | ---------------------- | ---------------------------------------------------- |
| 37  | Tickets List   | `/support/tickets`     | List of support tickets with priority/status filters |
| 38  | Ticket Details | `/support/tickets/:id` | Ticket view with comment thread                      |
| 39  | Create Ticket  | `/support/tickets/new` | Form to submit new support ticket                    |

---

## Module J: Administration & Settings (3 pages)

| #   | Page             | Route               | Description                                                     |
| --- | ---------------- | ------------------- | --------------------------------------------------------------- |
| 40  | User Management  | `/admin/users`      | CRUD for system users, assign roles (admin, manager, staff)     |
| 41  | Audit Logs       | `/admin/audit-logs` | Track system changes and user actions                           |
| 42  | Company Settings | `/admin/settings`   | Configure company name, tax rates, fiscal year, email templates |

---

## Page Summary

| Module                 | Page Count   |
| ---------------------- | ------------ |
| Dashboard & Common     | 2            |
| Inventory Management   | 6            |
| Sales & Customers      | 5            |
| Purchasing & Suppliers | 4            |
| Finance & Accounting   | 6            |
| Human Resources        | 4            |
| CRM                    | 4            |
| Project Management     | 3            |
| Helpdesk / Support     | 3            |
| Administration         | 3            |
| **Total**              | **40 pages** |

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

/crm
/crm/leads
/crm/leads/:id
/crm/pipeline

/projects
/projects/:id
/projects/:id/gantt

/support/tickets
/support/tickets/new
/support/tickets/:id
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
│   ├── crm/
│   │   ├── CRMDashboard.tsx
│   │   ├── LeadsList.tsx
│   │   ├── LeadDetails.tsx
│   │   └── Pipeline.tsx
│   ├── projects/
│   │   ├── ProjectsList.tsx
│   │   ├── ProjectDetails.tsx
│   │   └── GanttView.tsx
│   ├── support/
│   │   ├── TicketsList.tsx
│   │   ├── TicketDetails.tsx
│   │   └── TicketForm.tsx
│   ├── admin/
│   │   ├── UserManagement.tsx
│   │   ├── CompanySettings.tsx
│   │   └── AuditLogs.tsx
```

---

## Reusable Components Needed

| Component        | Usage                                           |
| ---------------- | ----------------------------------------------- |
| DataTable        | All list pages (sorting, filtering, pagination) |
| FormModal        | Quick create/edit in list views                 |
| StatCard         | Dashboard widgets                               |
| ChartContainer   | Dashboard charts                                |
| ProductSelector  | Sales order, purchase order forms               |
| CustomerSelector | Sales order form                                |
| SupplierSelector | Purchase order form                             |
| LineItemTable    | Order forms (dynamic rows)                      |
| CurrencyInput    | All monetary fields                             |
| DateRangePicker  | Filters and reports                             |
| StatusBadge      | Order status, invoice status                    |
| TabPanel         | Detail pages with multiple sections             |
| FileUpload       | Product images, documents                       |
| ApprovalWorkflow | Leave requests, purchase orders                 |
