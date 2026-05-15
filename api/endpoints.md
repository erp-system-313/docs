# API Endpoints

Base URL: `/api/v1`

## Authentication

| Method | Path | Description |
|--------|------|-------------|
| POST | `/auth/login` | Login with email/password |
| POST | `/auth/logout` | Logout |
| POST | `/auth/refresh` | Refresh access token |
| POST | `/auth/forgot-password` | Request password reset |
| POST | `/auth/reset-password` | Reset password with token |

## Dashboard

| Method | Path | Description |
|--------|------|-------------|
| GET | `/dashboard/stats` | Get dashboard statistics |

## Customers

| Method | Path | Description |
|--------|------|-------------|
| GET | `/customers` | List customers (paginated) |
| GET | `/customers/{id}` | Get customer by ID |
| POST | `/customers` | Create customer |
| PUT | `/customers/{id}` | Update customer |
| DELETE | `/customers/{id}` | Delete customer |
| GET | `/customers/{id}/orders` | Get orders for customer |

## Sales Orders

| Method | Path | Description |
|--------|------|-------------|
| GET | `/sales-orders` | List sales orders (paginated) |
| GET | `/sales-orders/{id}` | Get sales order by ID |
| POST | `/sales-orders` | Create sales order |
| PUT | `/sales-orders/{id}` | Update sales order |
| DELETE | `/sales-orders/{id}` | Delete sales order |
| PUT | `/sales-orders/{id}/confirm` | Confirm sales order |
| PUT | `/sales-orders/{id}/ship` | Mark as shipped |
| PUT | `/sales-orders/{id}/cancel` | Cancel sales order |

## Products

| Method | Path | Description |
|--------|------|-------------|
| GET | `/products` | List products (paginated) |
| GET | `/products/{id}` | Get product by ID |
| GET | `/products/low-stock` | Get low stock products |
| POST | `/products` | Create product |
| PUT | `/products/{id}` | Update product |
| DELETE | `/products/{id}` | Delete product |

## Categories

| Method | Path | Description |
|--------|------|-------------|
| GET | `/categories` | List categories (paginated) |
| GET | `/categories/{id}` | Get category by ID |
| POST | `/categories` | Create category |
| PUT | `/categories/{id}` | Update category |
| DELETE | `/categories/{id}` | Delete category |

## Suppliers

| Method | Path | Description |
|--------|------|-------------|
| GET | `/suppliers` | List suppliers (paginated) |
| GET | `/suppliers/{id}` | Get supplier by ID |
| POST | `/suppliers` | Create supplier |
| PUT | `/suppliers/{id}` | Update supplier |
| DELETE | `/suppliers/{id}` | Delete supplier |

## Purchase Orders

| Method | Path | Description |
|--------|------|-------------|
| GET | `/purchase-orders` | List purchase orders (paginated) |
| GET | `/purchase-orders/{id}` | Get purchase order by ID |
| POST | `/purchase-orders` | Create purchase order |
| PUT | `/purchase-orders/{id}` | Update purchase order |
| DELETE | `/purchase-orders/{id}` | Delete purchase order |
| PUT | `/purchase-orders/{id}/receive` | Receive inventory for PO |
| PUT | `/purchase-orders/{id}/cancel` | Cancel purchase order |

## Stock Movements

| Method | Path | Description |
|--------|------|-------------|
| GET | `/stock-movements` | List stock movements (paginated) |
| GET | `/stock-movements/{id}` | Get stock movement by ID |
| POST | `/stock-movements` | Create stock movement |

## Invoices

| Method | Path | Description |
|--------|------|-------------|
| GET | `/invoices` | List invoices (paginated) |
| GET | `/invoices/{id}` | Get invoice by ID |
| GET | `/invoices/{id}/pdf` | Get invoice PDF |
| POST | `/invoices` | Create invoice |
| PUT | `/invoices/{id}` | Update invoice |
| DELETE | `/invoices/{id}` | Delete invoice |
| GET | `/invoices/{id}/payments` | List payments for invoice |
| POST | `/invoices/{id}/payments` | Record payment |
| PUT | `/invoices/{id}/send` | Send invoice |
| PUT | `/invoices/{id}/cancel` | Cancel invoice |

## Journal Entries

| Method | Path | Description |
|--------|------|-------------|
| GET | `/journal-entries` | List journal entries (paginated) |
| GET | `/journal-entries/{id}` | Get journal entry by ID |
| POST | `/journal-entries` | Create journal entry |
| PUT | `/journal-entries/{id}` | Update journal entry |
| POST | `/journal-entries/{id}/post` | Post journal entry |
| PUT | `/journal-entries/{id}/reverse` | Reverse journal entry |

## Accounts

| Method | Path | Description |
|--------|------|-------------|
| GET | `/accounts` | List accounts (paginated, filterable by type) |
| GET | `/accounts/{id}` | Get account by ID |
| GET | `/accounts/type/{type}` | Get accounts by type (list) |
| POST | `/accounts` | Create account |
| PUT | `/accounts/{id}` | Update account |
| DELETE | `/accounts/{id}` | Delete account |

## Employees

| Method | Path | Description |
|--------|------|-------------|
| GET | `/employees` | List employees (paginated) |
| GET | `/employees/{id}` | Get employee by ID |
| POST | `/employees` | Create employee |
| PUT | `/employees/{id}` | Update employee |
| DELETE | `/employees/{id}` | Delete employee |
| GET | `/employees/{id}/attendance` | Get attendance for employee |

## Attendance

| Method | Path | Description |
|--------|------|-------------|
| GET | `/attendance` | List attendance records (paginated) |
| GET | `/attendance/{id}` | Get attendance by ID |
| POST | `/attendance/clock-in` | Clock in |
| POST | `/attendance/clock-out` | Clock out |
| DELETE | `/attendance/{id}` | Delete attendance record |

## Leave Requests

| Method | Path | Description |
|--------|------|-------------|
| GET | `/leave-requests` | List leave requests (paginated) |
| GET | `/leave-requests/{id}` | Get leave request by ID |
| POST | `/leave-requests` | Create leave request |
| PUT | `/leave-requests/{id}/approve` | Approve leave request |
| PUT | `/leave-requests/{id}/reject` | Reject leave request |
| GET | `/leave-requests/balances` | Get leave balances summary grouped by type |

## Leave Balances

| Method | Path | Description |
|--------|------|-------------|
| GET | `/leave-balances` | List leave balances (paginated) |

## Users

| Method | Path | Description |
|--------|------|-------------|
| GET | `/users` | List users (paginated) |
| GET | `/users/{id}` | Get user by ID |
| GET | `/users/email/{email}` | Find user by email |
| POST | `/users` | Create user |
| PUT | `/users/{id}` | Update user |
| DELETE | `/users/{id}` | Delete user |

## Settings

| Method | Path | Description |
|--------|------|-------------|
| GET | `/settings` | Get all company settings |
| GET | `/settings/{key}` | Get setting by key |
| PUT | `/settings` | Update company settings |
| DELETE | `/settings/{key}` | Delete a setting |

## Audit Logs

| Method | Path | Description |
|--------|------|-------------|
| GET | `/audit-logs` | List audit logs (paginated) |

---
*Last audited: 2026-05-10*  
**Note:** DELETE `/journal-entries/{id}` is defined in frontend `endpoints.ts` but no matching `@DeleteMapping` exists in backend `JournalEntryController`. Implement or remove endpoint.
