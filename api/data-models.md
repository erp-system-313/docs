# Data Models

## Authentication

### User
| Field | Type | Notes |
|-------|------|-------|
| id | Long | Auto-generated |
| email | String | Unique, not null |
| passwordHash | String | BCrypt encoded |
| firstName | String | Nullable |
| lastName | String | Nullable |
| role | Role | Many-to-one (entity ref, not enum) |
| employee | Employee | Optional, one-to-one |
| isActive | Boolean | Default true |
| lastLoginAt | LocalDateTime | Nullable |
| resetToken | String | Nullable, for password reset |
| resetTokenExpiresAt | LocalDateTime | Nullable |
| createdAt | LocalDateTime | Auto-set via @CreationTimestamp |
| updatedAt | LocalDateTime | Auto-set via @UpdateTimestamp |

UserDto adds `fullName` (computed), `roleName`, `roleId`, `employeeId` fields.

### Role
| Field | Type | Notes |
|-------|------|-------|
| id | Long | Auto-generated |
| name | String | Unique |
| description | String | |
| permissions | String (JSONB) | Stored as JSON |
| isActive | Boolean | |
| createdAt | LocalDateTime | |
| updatedAt | LocalDateTime | |

### AuditLog
| Field | Type | Notes |
|-------|------|-------|
| id | Long | |
| user | User | Many-to-one |
| action | String | e.g. CREATE, UPDATE, DELETE |
| entityType | String | e.g. Product, Invoice |
| entityId | Long | |
| changes | String (JSONB) | Before/after values |
| ipAddress | String | |
| details | String | |
| createdAt | LocalDateTime | |

### Settings
| Field | Type | Notes |
|-------|------|-------|
| id | Long | |
| settingKey | String | Unique |
| settingValue | String | Nullable |
| settingType | String | STRING (default), TEXT, NUMBER, BOOLEAN, JSON |
| description | String | Nullable |
| createdAt | LocalDateTime | Auto-set |

---

## Inventory

### Product
| Field | Type | Notes |
|-------|------|-------|
| id | Long | |
| sku | String | Unique |
| name | String | |
| description | String | |
| category | Category | Many-to-one |
| supplier | Supplier | Many-to-one, nullable |
| unitPrice | BigDecimal | |
| costPrice | BigDecimal | |
| reorderLevel | Integer | |
| reorderQuantity | Integer | |
| unitOfMeasure | String | e.g. pcs, kg, L |
| currentStock | Integer | Not null, default 0 |
| imageUrl | String | Nullable |
| isActive | Boolean | Default true |
| createdAt | LocalDateTime | |
| updatedAt | LocalDateTime | |

### Category
| Field | Type | Notes |
|-------|------|-------|
| id | Long | |
| name | String | |
| description | String | |
| parentId | Long | Self-referencing, nullable |
| sortOrder | Integer | |
| isActive | Boolean | Default true |
| productCount | Long | Count of active products in category |
| createdAt | LocalDateTime | |
| updatedAt | LocalDateTime | |

---

## Sales

### Customer
| Field | Type | Notes |
|-------|------|-------|
| id | Long | |
| name | String | |
| email | String | |
| phone | String | |
| address | String | |
| creditLimit | BigDecimal | |
| paymentTerms | String | NET_30, NET_60, IMMEDIATE |
| isActive | Boolean | Default true |
| createdAt | LocalDateTime | |
| updatedAt | LocalDateTime | |

### SalesOrder
| Field | Type | Notes |
|-------|------|-------|
| id | Long | |
| orderNumber | String | Unique |
| customer | Customer | Many-to-one |
| orderDate | LocalDateTime | |
| status | OrderStatus | DRAFT, CONFIRMED, SHIPPED, INVOICED, CANCELLED |
| subtotal | BigDecimal | |
| taxAmount | BigDecimal | |
| totalAmount | BigDecimal | |
| notes | String | Nullable |
| createdBy | User | Many-to-one |
| lines | List&lt;SalesOrderLine&gt; | One-to-many |
| createdAt | LocalDateTime | |
| updatedAt | LocalDateTime | |

### SalesOrderLine
| Field | Type | Notes |
|-------|------|-------|
| id | Long | |
| order | SalesOrder | Many-to-one |
| product | Product | Many-to-one |
| quantity | Integer | |
| unitPrice | BigDecimal | |
| lineTotal | BigDecimal | |
| description | String | Nullable |

---

## Purchasing

### Supplier
| Field | Type | Notes |
|-------|------|-------|
| id | Long | |
| code | String | Unique |
| name | String | |
| contactPerson | String | |
| email | String | |
| phone | String | |
| address | String | |
| taxId | String | Nullable |
| paymentTerms | Integer | Days (e.g. 30, 60) |
| totalPurchased | BigDecimal | |
| isActive | Boolean | Default true |
| createdAt | LocalDateTime | |
| updatedAt | LocalDateTime | |

### PurchaseOrder
| Field | Type | Column | Notes |
|-------|------|--------|-------|
| id | Long | id | |
| poNumber | String | po_number | Unique |
| supplier | Supplier | supplier_id | Many-to-one |
| orderDate | LocalDateTime | order_date | |
| status | Status | status | DRAFT, SENT, RECEIVED, PARTIAL, CANCELLED |
| subtotal | BigDecimal | subtotal | |
| taxAmount | BigDecimal | tax_amount | |
| totalAmount | BigDecimal | total_amount | |
| shippingCost | BigDecimal | shipping_cost | |
| expectedDate | LocalDate | expected_date | |
| receivedDate | LocalDate | received_date | Nullable |
| notes | String | notes | |
| createdBy | User | created_by | Many-to-one |
| lines | List&lt;PurchaseOrderLine&gt; | | One-to-many |
| createdAt | LocalDateTime | created_at | |
| updatedAt | LocalDateTime | updated_at | |

### PurchaseOrderLine
| Field | Type | Notes |
|-------|------|-------|
| id | Long | |
| purchaseOrder | PurchaseOrder | Many-to-one |
| product | Product | Many-to-one |
| quantity | Integer | |
| unitPrice | BigDecimal | |
| discount | BigDecimal | |
| lineTotal | BigDecimal | |
| receivedQty | Integer | |
| notes | String | Nullable |

### StockMovement
| Field | Type | Column | Notes |
|-------|------|--------|-------|
| id | Long | id | |
| product | Product | product_id | Many-to-one |
| type | MovementType | movement_type | IN, OUT, ADJUSTMENT |
| quantity | Integer | quantity | |
| previousStock | Integer | previous_stock | |
| newStock | Integer | new_stock | |
| referenceType | String | reference_type | PO, SO, ADJUSTMENT |
| referenceId | Long | reference_id | |
| date | LocalDate | movement_date | |
| notes | String | notes | |
| createdBy | User | created_by | Many-to-one |
| createdAt | LocalDateTime | created_at | |

---

## Finance

### Invoice
| Field | Type | Column | Notes |
|-------|------|--------|-------|
| id | Long | id | |
| invoiceNumber | String | invoice_number | Unique |
| customer | Customer | customer_id | Many-to-one |
| invoiceDate | LocalDateTime | issue_date | |
| dueDate | LocalDateTime | due_date | |
| dueAt | LocalDateTime | due_at | Nullable |
| status | InvoiceStatus | status | DRAFT, SENT, PAID, OVERDUE, CANCELLED |
| subtotal | BigDecimal | subtotal | |
| taxAmount | BigDecimal | tax_amount | |
| total | BigDecimal | total_amount | |
| paidAmount | BigDecimal | paid_amount | |
| balance | BigDecimal | (computed) | total - paidAmount |
| salesOrder | SalesOrder | sales_order_id | Many-to-one, nullable |
| payments | List&lt;Payment&gt; | | One-to-many |
| lines | List&lt;InvoiceLine&gt; | | One-to-many |
| sentAt | LocalDateTime | sent_at | Nullable |
| createdAt | LocalDateTime | created_at | |
| updatedAt | LocalDateTime | updated_at | |

### InvoiceLine
| Field | Type | Notes |
|-------|------|-------|
| id | Long | |
| invoice | Invoice | Many-to-one |
| product | Product | Many-to-one, nullable |
| description | String | Nullable |
| quantity | Integer | Not null |
| unitPrice | BigDecimal | Not null |
| lineTotal | BigDecimal | Not null |
| glAccountId | Long | Nullable |
| taxCode | String | Nullable |
| taxRate | BigDecimal | Nullable |

### Payment
| Field | Type | Notes |
|-------|------|-------|
| id | Long | |
| invoice | Invoice | Many-to-one |
| amount | BigDecimal | |
| paymentDate | LocalDateTime | |
| method | PaymentMethod | CASH, CARD, BANK_TRANSFER, CHEQUE |
| reference | String | Nullable |
| notes | String | Nullable |
| createdAt | LocalDateTime | |

### Account
| Field | Type | Notes |
|-------|------|-------|
| id | Long | |
| code | String | Unique |
| name | String | |
| type | AccountType | ASSET, LIABILITY, EQUITY, INCOME, EXPENSE |
| parent | Account | Self-referencing, nullable |
| balance | BigDecimal | |
| isActive | Boolean | Default true |
| createdAt | LocalDateTime | |
| updatedAt | LocalDateTime | |

### JournalEntry
| Field | Type | Column | Notes |
|-------|------|--------|-------|
| id | Long | id | |
| entryNumber | String | entry_number | Unique |
| date | LocalDate | entry_date | |
| journalType | String | journal_type | MISC (default), SALES, PURCHASES, etc. |
| description | String | description | |
| reference | String | reference | Nullable |
| status | JournalEntryStatus | status | DRAFT, POSTED |
| createdBy | User | created_by | Many-to-one |
| lines | List&lt;JournalEntryLine&gt; | | One-to-many |
| postedAt | LocalDateTime | posted_at | Nullable |
| createdAt | LocalDateTime | created_at | |

### JournalEntryLine
| Field | Type | Notes |
|-------|------|-------|
| id | Long | |
| entry | JournalEntry | Many-to-one |
| account | Account | Many-to-one |
| debit | BigDecimal | |
| credit | BigDecimal | |
| description | String | Nullable |

---

## HR

### Employee
| Field | Type | Notes |
|-------|------|-------|
| id | Long | |
| user | User | One-to-one, nullable |
| employeeCode | String | Unique |
| firstName | String | |
| lastName | String | |
| email | String | Unique |
| phone | String | |
| department | String | |
| position | String | |
| hireDate | LocalDate | |
| terminationDate | LocalDate | Nullable |
| salary | BigDecimal | |
| status | EmployeeStatus | ACTIVE, INACTIVE, TERMINATED |
| address | String | |
| emergencyContact | String | |
| emergencyPhone | String | |
| createdAt | LocalDateTime | |
| updatedAt | LocalDateTime | |

### Attendance
| Field | Type | Notes |
|-------|------|-------|
| id | Long | |
| employee | Employee | Many-to-one |
| date | LocalDate | |
| checkIn | LocalDateTime | |
| checkOut | LocalDateTime | Nullable |
| status | AttendanceStatus | PRESENT, ABSENT, LATE, LEAVE |
| notes | String | Nullable |
| createdAt | LocalDateTime | |

### LeaveRequest
| Field | Type | Notes |
|-------|------|-------|
| id | Long | |
| employee | Employee | Many-to-one |
| startDate | LocalDate | |
| endDate | LocalDate | |
| type | LeaveType | ANNUAL, SICK, PERSONAL, UNPAID |
| status | LeaveStatus | PENDING, APPROVED, REJECTED, CANCELLED |
| reason | String | |
| rejectionReason | String | Nullable |
| approvedBy | User | Many-to-one, nullable |
| approvedAt | LocalDateTime | Nullable |
| createdAt | LocalDateTime | |
| updatedAt | LocalDateTime | |

### LeaveBalance
| Field | Type | Notes |
|-------|------|-------|
| id | Long | |
| employee | Employee | Many-to-one |
| type | LeaveType | ANNUAL, SICK, PERSONAL, UNPAID |
| totalDays | int | |
| usedDays | int | |
| year | int | |
| createdAt | LocalDateTime | |
| updatedAt | LocalDateTime | |

---
*Last audited: 2026-05-10*

### Changelog (2026-05-10)
| # | Change | Detail |
|---|--------|--------|
| 1 | Fixed PurchaseOrder.expectedDate | Column name corrected from `delivery_date` to `expected_date` to match V4 migration |
| 2 | Fixed JournalEntry.date | Column name corrected from `date` to `entry_date` to match V6 migration |
| 3 | Fixed StockMovement.date | Column name corrected from `date` to `movement_date` to match V4 migration |
| 4 | Fixed Attendance.date | Column name corrected from `date` to `attendance_date` to match V2 migration |
| 5 | Added missing fields to PurchaseOrder | Added `shippingCost`, `receivedDate` with their DB columns |
| 6 | Added missing fields to Invoice | Added `balance` (computed), confirmed `sentAt`, `dueAt` columns |
| 7 | Added missing fields to StockMovement | Added `previousStock`, `newStock`, `referenceType`, `referenceId` |
| 8 | Added Supplier fields | `code`, `taxId`, `totalPurchased` documented (added via V18 migration) |
