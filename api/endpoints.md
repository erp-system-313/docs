# API Endpoints

# API Endpoints

## Base URL

```
https://api.example.com/api/v1
```

## Conventions

| Convention  | Value                             |
| ----------- | --------------------------------- |
| Versioning  | URL-based (`/v1`)                 |
| Pagination  | `?page=0&size=20`                 |
| Date Format | ISO 8601 (`2026-03-26T10:30:00Z`) |
| ID Format   | Auto-increment integer            |

---

## Common Responses

### Success Response

```json
{
  "success": true,
  "message": "Success",
  "data": { ... },
  "timestamp": "2026-03-26T10:30:00Z"
}
```

### Paginated Response

```json
{
  "success": true,
  "data": {
    "content": [ ... ],
    "totalElements": 100,
    "totalPages": 5,
    "size": 20,
    "number": 0,
    "first": true,
    "last": false
  }
}
```

### Error Response

```json
{
  "success": false,
  "error": {
    "code": "ERR_CODE",
    "message": "Error message",
    "fieldErrors": []
  },
  "timestamp": "2026-03-26T10:30:00Z"
}
```

---

## Common Query Parameters

| Parameter | Type   | Description                         |
| --------- | ------ | ----------------------------------- |
| `page`    | int    | Page number (default: 0)            |
| `size`    | int    | Page size (default: 20, max: 100)   |
| `sort`    | string | Sort field (e.g., `createdAt,desc`) |
| `search`  | string | Search query                        |
| `filter`  | string | Filter expression                   |

---

## Auth Module

### POST /auth/login

Login with credentials.

**Request:**

```json
{
  "email": "user@example.com",
  "password": "password123"
}
```

**Response:** `200 OK`

```json
{
  "success": true,
  "data": {
    "accessToken": "eyJhbG...",
    "refreshToken": "eyJhbG...",
    "expiresIn": 900,
    "user": {
      "id": 1,
      "email": "user@example.com",
      "name": "John Doe",
      "role": "ADMIN"
    }
  }
}
```

### POST /auth/refresh

Refresh access token.

**Request:**

```json
{
  "refreshToken": "eyJhbG..."
}
```

**Response:** `200 OK`

```json
{
  "success": true,
  "data": {
    "accessToken": "eyJhbG...",
    "expiresIn": 900
  }
}
```

### POST /auth/logout

Logout and invalidate token.

**Headers:** `Authorization: Bearer <token>`

**Response:** `200 OK`

### POST /auth/forgot-password

Request password reset.

**Request:**

```json
{
  "email": "user@example.com"
}
```

**Response:** `200 OK`

### POST /auth/reset-password

Reset password with token.

**Request:**

```json
{
  "token": "reset-token",
  "newPassword": "newpassword123"
}
```

**Response:** `200 OK`

---

## Users Module

### GET /users

Get all users (Admin only).

**Query Parameters:**

- `page`, `size`, `sort`
- `role` (filter by role)
- `isActive` (filter)

**Response:** `200 OK` - Paginated users

### GET /users/{id}

Get user by ID.

**Response:** `200 OK` - User object

### POST /users

Create new user (Admin only).

**Request:**

```json
{
  "email": "newuser@example.com",
  "name": "New User",
  "password": "password123",
  "role": "STAFF",
  "employeeId": 1
}
```

**Response:** `201 Created`

### PUT /users/{id}

Update user.

**Request:**

```json
{
  "name": "Updated Name",
  "role": "MANAGER",
  "isActive": true
}
```

**Response:** `200 OK`

### DELETE /users/{id}

Deactivate user (soft delete).

**Response:** `204 No Content`

---

## Dashboard Module

### GET /dashboard/stats

Get dashboard statistics.

**Response:** `200 OK`

```json
{
  "success": true,
  "data": {
    "totalSales": 150000,
    "totalPurchases": 80000,
    "pendingOrders": 12,
    "pendingInvoices": 5,
    "lowStockProducts": 3,
    "totalEmployees": 25,
    "salesTrend": [...],
    "topProducts": [...],
    "recentOrders": [...]
  }
}
```

---

## Products Module

### GET /products

Get all products.

**Query Parameters:**

- `page`, `size`, `sort`
- `categoryId`
- `supplierId`
- `search` (search by name/SKU)
- `isActive`

**Response:** `200 OK` - Paginated products

### GET /products/{id}

Get product by ID.

### GET /products/low-stock

Get products with low stock (below reorder level).

**Response:** `200 OK` - List of products

### POST /products

Create new product.

**Request:**

```json
{
  "name": "Product Name",
  "sku": "SKU-001",
  "categoryId": 1,
  "unitPrice": 99.99,
  "costPrice": 50.0,
  "reorderLevel": 10,
  "description": "Description",
  "imageUrl": "https://..."
}
```

**Response:** `201 Created`

### PUT /products/{id}

Update product.

**Response:** `200 OK`

### DELETE /products/{id}

Delete product (soft delete).

**Response:** `204 No Content`

---

## Categories Module

### GET /categories

Get all categories.

**Query Parameters:**

- `page`, `size`, `sort`
- `parentId` (for nested)

**Response:** `200 OK` - Paginated categories

### GET /categories/{id}

Get category with products.

### POST /categories

Create category.

**Request:**

```json
{
  "name": "Electronics",
  "description": "Electronic items",
  "parentId": null
}
```

**Response:** `201 Created`

### PUT /categories/{id}

Update category.

### DELETE /categories/{id}

Delete category.

---

## Suppliers Module

### GET /suppliers

Get all suppliers.

**Query Parameters:**

- `page`, `size`, `sort`
- `search`
- `isActive`

**Response:** `200 OK` - Paginated suppliers

### GET /suppliers/{id}

Get supplier with purchase orders.

### POST /suppliers

Create supplier.

**Request:**

```json
{
  "name": "Supplier Name",
  "contactPerson": "John Doe",
  "email": "supplier@example.com",
  "phone": "+1234567890",
  "address": "Address"
}
```

**Response:** `201 Created`

### PUT /suppliers/{id}

Update supplier.

### DELETE /suppliers/{id}

Soft delete supplier.

---

## Sales Orders Module

### GET /sales-orders

Get all sales orders.

**Query Parameters:**

- `page`, `size`, `sort`
- `status` (DRAFT, CONFIRMED, SHIPPED, INVOICED, CANCELLED)
- `customerId`
- `dateFrom`, `dateTo`

**Response:** `200 OK` - Paginated orders

### GET /sales-orders/{id}

Get sales order with line items.

### POST /sales-orders

Create sales order.

**Request:**

```json
{
  "customerId": 1,
  "orderDate": "2026-03-26T10:30:00Z",
  "notes": "Order notes",
  "lines": [
    {
      "productId": 1,
      "quantity": 5,
      "unitPrice": 99.99
    }
  ]
}
```

**Response:** `201 Created`

### PUT /sales-orders/{id}

Update sales order (only DRAFT).

### PUT /sales-orders/{id}/confirm

Confirm sales order.

**Response:** `200 OK`

### PUT /sales-orders/{id}/ship

Mark as shipped.

### PUT /sales-orders/{id}/cancel

Cancel sales order.

### DELETE /sales-orders/{id}

Delete sales order (only DRAFT).

**Response:** `204 No Content`

---

## Customers Module

### GET /customers

Get all customers.

**Query Parameters:**

- `page`, `size`, `sort`
- `search`
- `isActive`

**Response:** `200 OK` - Paginated customers

### GET /customers/{id}

Get customer with order history.

### GET /customers/{id}/orders

Get customer's orders.

### POST /customers

Create customer.

**Request:**

```json
{
  "name": "Customer Name",
  "email": "customer@example.com",
  "phone": "+1234567890",
  "address": "Address",
  "creditLimit": 10000,
  "paymentTerms": "NET_30"
}
```

**Response:** `201 Created`

### PUT /customers/{id}

Update customer.

### DELETE /customers/{id}

Soft delete customer.

---

## Invoices Module

### GET /invoices

Get all invoices.

**Query Parameters:**

- `page`, `size`, `sort`
- `status` (DRAFT, SENT, PAID, OVERDUE, CANCELLED)
- `customerId`
- `dateFrom`, `dateTo`

**Response:** `200 OK` - Paginated invoices

### GET /invoices/{id}

Get invoice with payments.

### GET /invoices/{id}/pdf

Get invoice as PDF.

**Response:** `200 OK` - PDF binary

### POST /invoices

Create invoice (manual or from sales order).

**Request:**

```json
{
  "salesOrderId": 1,
  "invoiceDate": "2026-03-26T10:30:00Z",
  "dueDate": "2026-04-25T10:30:00Z"
}
```

**Response:** `201 Created`

### POST /invoices/{id}/payments

Record payment.

**Request:**

```json
{
  "amount": 500.0,
  "paymentDate": "2026-03-26T10:30:00Z",
  "method": "CASH",
  "reference": "REF-001"
}
```

**Response:** `200 OK`

### PUT /invoices/{id}/send

Mark as sent.

### PUT /invoices/{id}/cancel

Cancel invoice.

---

## Purchase Orders Module

### GET /purchase-orders

Get all purchase orders.

**Query Parameters:**

- `page`, `size`, `sort`
- `status` (DRAFT, SENT, RECEIVED, CANCELLED)
- `supplierId`

**Response:** `200 OK` - Paginated orders

### GET /purchase-orders/{id}

Get purchase order with line items.

### POST /purchase-orders

Create purchase order.

**Request:**

```json
{
  "supplierId": 1,
  "orderDate": "2026-03-26T10:30:00Z",
  "expectedDate": "2026-04-01T10:30:00Z",
  "lines": [
    {
      "productId": 1,
      "quantity": 10,
      "unitPrice": 50.0
    }
  ]
}
```

**Response:** `201 Created`

### PUT /purchase-orders/{id}/receive

Record goods receipt.

**Request:**

```json
{
  "lines": [
    {
      "lineId": 1,
      "receivedQty": 10
    }
  ]
}
```

**Response:** `200 OK`

### PUT /purchase-orders/{id}/cancel

Cancel purchase order.

---

## Accounts Module

### GET /accounts

Get chart of accounts.

**Query Parameters:**

- `page`, `size`, `sort`
- `type` (ASSET, LIABILITY, EQUITY, INCOME, EXPENSE)
- `parentId`

**Response:** `200 OK` - Paginated accounts

### GET /accounts/{id}

Get account with transactions.

### POST /accounts

Create account.

**Request:**

```json
{
  "code": "4000",
  "name": "Revenue",
  "type": "INCOME",
  "parentId": null
}
```

**Response:** `201 Created`

### PUT /accounts/{id}

Update account.

### DELETE /accounts/{id}

Delete account (only if no transactions).

---

## Journal Module

### GET /journal-entries

Get journal entries.

**Query Parameters:**

- `page`, `size`, `sort`
- `status` (DRAFT, POSTED)
- `dateFrom`, `dateTo`

**Response:** `200 OK` - Paginated entries

### GET /journal-entries/{id}

Get journal entry with lines.

### POST /journal-entries

Create journal entry.

**Request:**

```json
{
  "date": "2026-03-26T10:30:00Z",
  "description": "Entry description",
  "reference": "REF-001",
  "lines": [
    {
      "accountId": 1,
      "debit": 1000.0,
      "credit": 0
    },
    {
      "accountId": 2,
      "debit": 0,
      "credit": 1000.0
    }
  ]
}
```

**Response:** `201 Created`

### POST /journal-entries/{id}/post

Post journal entry.

### PUT /journal-entries/{id}/reverse

Reverse journal entry.

---

## Employees Module

### GET /employees

Get all employees.

**Query Parameters:**

- `page`, `size`, `sort`
- `department`
- `status`

**Response:** `200 OK` - Paginated employees

### GET /employees/{id}

Get employee details.

### GET /employees/{id}/attendance

Get attendance records.

**Query Parameters:**

- `dateFrom`, `dateTo`

### POST /employees

Create employee.

**Request:**

```json
{
  "firstName": "John",
  "lastName": "Doe",
  "email": "john@example.com",
  "phone": "+1234567890",
  "department": "Engineering",
  "position": "Developer",
  "hireDate": "2026-01-15",
  "salary": 50000
}
```

**Response:** `201 Created`

### PUT /employees/{id}

Update employee.

### DELETE /employees/{id}

Terminate employee.

---

## Attendance Module

### GET /attendance

Get attendance records.

**Query Parameters:**

- `page`, `size`
- `employeeId`
- `dateFrom`, `dateTo`

### POST /attendance/clock-in

Clock in.

**Response:** `200 OK`

### POST /attendance/clock-out

Clock out.

**Response:** `200 OK`

---

## Leave Module

### GET /leave-requests

Get leave requests.

**Query Parameters:**

- `page`, `size`
- `employeeId`
- `status` (PENDING, APPROVED, REJECTED)
- `type` (ANNUAL, SICK, PERSONAL)

**Response:** `200 OK` - Paginated requests

### GET /leave-requests/{id}

Get leave request details.

### POST /leave-requests

Submit leave request.

**Request:**

```json
{
  "startDate": "2026-04-01",
  "endDate": "2026-04-05",
  "type": "ANNUAL",
  "reason": "Family vacation"
}
```

**Response:** `201 Created`

### PUT /leave-requests/{id}/approve

Approve leave request (Manager/Admin).

### PUT /leave-requests/{id}/reject

Reject leave request.

### GET /leave-balances

Get current user's leave balances.

**Response:** `200 OK`

```json
{
  "success": true,
  "data": {
    "annual": { "total": 21, "used": 5, "remaining": 16 },
    "sick": { "total": 10, "used": 2, "remaining": 8 }
  }
}
```

---

## Project Management Module

### GET /projects

Get all projects.

**Query Parameters:**

- `page`, `size`, `sort`
- `state` (PLANNING, ACTIVE, ON_HOLD, COMPLETED, CANCELLED)
- `search` (search by name)

**Response:** `200 OK` - Paginated projects

### GET /projects/{id}

Get project details.

**Response:** `200 OK`

### POST /projects

Create project.

**Request:**

```json
{
  "name": "New Website",
  "customerId": 1,
  "dateStart": "2026-04-01",
  "dateEnd": "2026-06-30",
  "budget": 50000.0
}
```

**Response:** `201 Created`

### PATCH /projects/{id}/state

Update project state.

**Request:**

```json
"ACTIVE"
```

**Response:** `200 OK`

### DELETE /projects/{id}

Delete project.

**Response:** `204 No Content`

### GET /projects/{id}/stages

Get task stages for a project.

**Response:** `200 OK` - List of stages

### GET /projects/{id}/tasks

Get tasks for a project.

**Response:** `200 OK` - List of tasks

### POST /projects/{id}/tasks

Create task in project.

**Request:**

```json
{
  "name": "Design homepage",
  "description": "Create wireframes and mockups",
  "assignedTo": 1,
  "stageId": 1,
  "dueDate": "2026-04-15",
  "estimatedHours": 20.0
}
```

**Response:** `201 Created`

### GET /projects/{id}/gantt

Get Gantt chart data.

**Response:** `200 OK` - List of tasks with stage/dates

### PUT /tasks/{id}

Update task.

**Request:**

```json
{
  "name": "Updated task name",
  "stageId": 2,
  "actualHours": 18.5
}
```

**Response:** `200 OK`

---

## CRM Module (Planned)

### GET /crm/dashboard

Get CRM dashboard stats.

**Response:** `200 OK`

```json
{
  "success": true,
  "data": {
    "totalLeads": 150,
    "conversionRate": 0.25,
    "pipelineValue": 500000,
    "wonThisMonth": 75000
  }
}
```

### GET /crm/leads

Get all leads.

**Query Parameters:**

- `page`, `size`, `sort`
- `status` (NEW, CONTACTED, QUALIFIED, CONVERTED, LOST)
- `search`

**Response:** `200 OK` - Paginated leads

### GET /crm/leads/{id}

Get lead details.

### POST /crm/leads

Create lead.

**Request:**

```json
{
  "name": "John Smith",
  "email": "john@example.com",
  "phone": "+1234567890",
  "company": "Acme Corp",
  "source": "WEBSITE",
  "assignedTo": 1,
  "notes": "Interested in ERP software"
}
```

**Response:** `201 Created`

### PUT /crm/leads/{id}

Update lead.

### POST /crm/leads/{id}/convert

Convert lead to customer.

**Response:** `200 OK`

### GET /crm/pipelines

Get pipeline stages.

**Response:** `200 OK` - List of stages with deal counts

### PUT /crm/opportunities/{id}/stage

Update opportunity stage.

**Request:**

```json
{
  "stageId": 2
}
```

**Response:** `200 OK`

---

## Helpdesk Module

### GET /support/tickets

Get all tickets (paginated, filterable).

**Query Parameters:**

- `page` (default: 0), `size` (default: 20)
- `status` (OPEN, IN_PROGRESS, RESOLVED, CLOSED)
- `priority` (LOW, MEDIUM, HIGH, URGENT)
- `customerId`
- `assignedToId`
- `createdById` — Filter by creator user ID
- `stageId` — Filter by workflow stage
- `teamId` — Filter by helpdesk team
- `categoryId` — Filter by ticket category
- `includeArchived` (boolean, default: false) — Include archived tickets

**Response:** `200 OK` - Paginated tickets

### GET /support/tickets/{id}

Get ticket with details (includes comments list).

**Response:** `200 OK`

```json
{
  "id": 1,
  "title": "Cannot login to system",
  "description": "Getting error 500",
  "customerId": 1,
  "customerName": "Acme Corp",
  "priority": "HIGH",
  "status": "OPEN",
  "assignedToId": null,
  "assignedToName": null,
  "createdById": 1,
  "createdByName": "John Doe",
  "stageId": 1,
  "stageName": "New",
  "teamId": 1,
  "teamName": "L1 Support",
  "categoryId": 1,
  "categoryName": "Login Issues",
  "channel": "PORTAL",
  "slaDeadline": "2026-05-12T07:00:00",
  "slaStatus": "OK",
  "closedAt": null,
  "isArchived": false,
  "tags": ["bug", "security"],
  "createdAt": "2026-05-10T07:00:00",
  "updatedAt": null,
  "comments": []
}
```

### POST /support/tickets

Create ticket.

**Request:**

```json
{
  "title": "Cannot login to system",
  "description": "Getting error 500 when trying to login",
  "customerId": 1,
  "priority": "HIGH",
  "assignedTo": null,
  "stageId": 1,
  "teamId": 1,
  "categoryId": 1,
  "channel": "PORTAL"
}
```

**Response:** `201 Created`

### PUT /support/tickets/{id}

Update ticket.

**Request:**

```json
{
  "title": "Cannot login to system — resolved",
  "description": "Updated description",
  "priority": "URGENT",
  "status": "IN_PROGRESS",
  "assignedTo": 1,
  "stageId": 2,
  "teamId": 1,
  "categoryId": 1
}
```

**Response:** `200 OK`

Note: Set `assignedTo` to `0` to unassign a ticket.

### DELETE /support/tickets/{id}

Delete ticket (hard delete, cascade removes comments).

**Response:** `204 No Content`

---

### POST /support/tickets/{id}/comments

Add a comment to a ticket.

**Request:**

```json
{
  "message": "Working on this now, will update soon.",
  "isInternal": false
}
```

**Response:** `201 Created`

### POST /support/tickets/{id}/close

Close a resolved ticket.

**Response:** `200 OK`

```json
{
  "id": 1,
  "status": "CLOSED",
  "closedAt": "2026-05-15T10:00:00"
}
```

### POST /support/tickets/{id}/assign

Assign or reassign a ticket.

**Request:**

```json
{
  "assignedTo": 2
}
```

**Response:** `200 OK`

```json
{
  "id": 1,
  "assignedToId": 2,
  "assignedToName": "Jane Smith"
}
```

### POST /support/tickets/{id}/stage

Move a ticket to a different workflow stage.

**Request:**

```json
{
  "stageId": 3
}
```

**Response:** `200 OK`

### GET /support/stats

Get helpdesk statistics.

**Response:** `200 OK`

```json
{
  "totalTickets": 150,
  "openTickets": 30,
  "overdueSla": 5,
  "avgResolutionTime": "12h"
}
```

---

## Settings Module

### GET /settings

Get company settings.

**Response:** `200 OK`

### PUT /settings

Update company settings.

**Request:**

```json
{
  "companyName": "Company Name",
  "taxRate": 0.14,
  "currency": "USD",
  "fiscalYearStart": "01-01"
}
```

**Response:** `200 OK`

---

## Audit Logs Module

### GET /audit-logs

Get audit logs (Admin only).

**Query Parameters:**

- `page`, `size`
- `userId`
- `action`
- `dateFrom`, `dateTo`

**Response:** `200 OK` - Paginated logs

---

## Errors

### HTTP Status Codes

| Code | Description           |
| ---- | --------------------- |
| 200  | Success               |
| 201  | Created               |
| 204  | No Content            |
| 400  | Bad Request           |
| 401  | Unauthorized          |
| 403  | Forbidden             |
| 404  | Not Found             |
| 409  | Conflict              |
| 422  | Validation Error      |
| 500  | Internal Server Error |

### Error Codes

| Code             | Description              |
| ---------------- | ------------------------ |
| UNAUTHORIZED     | Invalid or missing token |
| FORBIDDEN        | Insufficient permissions |
| NOT_FOUND        | Resource not found       |
| VALIDATION_ERROR | Invalid input            |
| DUPLICATE_ENTRY  | Resource already exists  |
| INTERNAL_ERROR   | Server error             |
