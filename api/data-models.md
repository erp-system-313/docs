# Data Models

## Overview

This document defines all database models with their fields, types, and constraints.

| Convention | Value                  |
| ---------- | ---------------------- |
| ID Type    | Auto-increment Integer |
| Timestamps | ISO 8601               |

---

## User

| Field        | Type        | Constraints             | Description      |
| ------------ | ----------- | ----------------------- | ---------------- |
| id           | Long        | PK, Auto-increment      | Primary key      |
| email        | String(255) | NOT NULL, UNIQUE, Email | User email       |
| passwordHash | String(255) | NOT NULL                | BCrypt hash      |
| roleId       | Long        | FK → Role               | User role        |
| employeeId   | Long        | FK → Employee, NULL     | Link to employee |
| isActive     | Boolean     | DEFAULT true            | Account status   |
| createdAt    | Timestamp   | NOT NULL                | Creation time    |
| updatedAt    | Timestamp   | NOT NULL                | Last update      |

---

## Role

| Field       | Type       | Constraints        | Description      |
| ----------- | ---------- | ------------------ | ---------------- |
| id          | Long       | PK, Auto-increment | Primary key      |
| name        | String(50) | NOT NULL, UNIQUE   | Role name        |
| permissions | JSON       | NOT NULL           | Permission array |

**Permissions Structure:**

```json
["USER_READ", "USER_CREATE", "USER_UPDATE", "USER_DELETE", ...]
```

---

## Employee

| Field      | Type             | Constraints        | Description                  |
| ---------- | ---------------- | ------------------ | ---------------------------- |
| id         | Long             | PK, Auto-increment | Primary key                  |
| userId     | Long             | FK → User, NULL    | Link to user account         |
| firstName  | String(100)      | NOT NULL           | First name                   |
| lastName   | String(100)      | NOT NULL           | Last name                    |
| email      | String(255)      | NOT NULL, UNIQUE   | Email                        |
| phone      | String(20)       | NULL               | Phone number                 |
| department | String(100)      | NULL               | Department                   |
| position   | String(100)      | NULL               | Job position                 |
| hireDate   | Date             | NOT NULL           | Hire date                    |
| salary     | BigDecimal(15,2) | NULL               | Salary                       |
| status     | Enum             | DEFAULT ACTIVE     | ACTIVE, INACTIVE, TERMINATED |
| createdAt  | Timestamp        | NOT NULL           | Creation time                |
| updatedAt  | Timestamp        | NOT NULL           | Last update                  |

---

## Category

| Field       | Type        | Constraints         | Description     |
| ----------- | ----------- | ------------------- | --------------- |
| id          | Long        | PK, Auto-increment  | Primary key     |
| name        | String(100) | NOT NULL            | Category name   |
| description | Text        | NULL                | Description     |
| parentId    | Long        | FK → Category, NULL | Parent category |
| createdAt   | Timestamp   | NOT NULL            | Creation time   |
| updatedAt   | Timestamp   | NOT NULL            | Last update     |

---

## Product

| Field        | Type          | Constraints         | Description        |
| ------------ | ------------- | ------------------- | ------------------ |
| id           | Long          | PK, Auto-increment  | Primary key        |
| sku          | String(50)    | NOT NULL, UNIQUE    | Stock keeping unit |
| name         | String(255)   | NOT NULL            | Product name       |
| description  | Text          | NULL                | Description        |
| categoryId   | Long          | FK → Category       | Category           |
| supplierId   | Long          | FK → Supplier, NULL | Primary supplier   |
| unitPrice    | Decimal(15,2) | NOT NULL            | Selling price      |
| costPrice    | Decimal(15,2) | NULL                | Cost price         |
| reorderLevel | Integer       | DEFAULT 10          | Reorder threshold  |
| currentStock | Integer       | DEFAULT 0           | Current quantity   |
| imageUrl     | String(500)   | NULL                | Product image      |
| isActive     | Boolean       | DEFAULT true        | Active status      |
| createdAt    | Timestamp     | NOT NULL            | Creation time      |
| updatedAt    | Timestamp     | NOT NULL            | Last update        |

---

## Supplier

| Field         | Type        | Constraints        | Description   |
| ------------- | ----------- | ------------------ | ------------- |
| id            | Long        | PK, Auto-increment | Primary key   |
| name          | String(255) | NOT NULL           | Supplier name |
| contactPerson | String(100) | NULL               | Contact name  |
| email         | String(255) | NULL               | Email         |
| phone         | String(20)  | NULL               | Phone         |
| address       | Text        | NULL               | Address       |
| isActive      | Boolean     | DEFAULT true       | Active status |
| createdAt     | Timestamp   | NOT NULL           | Creation time |
| updatedAt     | Timestamp   | NOT NULL           | Last update   |

---

## StockMovement

| Field         | Type       | Constraints        | Description         |
| ------------- | ---------- | ------------------ | ------------------- |
| id            | Long       | PK, Auto-increment | Primary key         |
| productId     | Long       | FK → Product       | Product             |
| type          | Enum       | NOT NULL           | IN, OUT, ADJUSTMENT |
| quantity      | Integer    | NOT NULL           | Movement quantity   |
| referenceType | String(50) | NULL               | Reference type      |
| referenceId   | Long       | NULL               | Reference ID        |
| notes         | Text       | NULL               | Notes               |
| createdAt     | Timestamp  | NOT NULL           | Creation time       |

---

## Customer

| Field        | Type          | Constraints        | Description               |
| ------------ | ------------- | ------------------ | ------------------------- |
| id           | Long          | PK, Auto-increment | Primary key               |
| name         | String(255)   | NOT NULL           | Customer name             |
| email        | String(255)   | NULL               | Email                     |
| phone        | String(20)    | NULL               | Phone                     |
| address      | Text          | NULL               | Address                   |
| creditLimit  | Decimal(15,2) | DEFAULT 0          | Credit limit              |
| paymentTerms | String(20)    | DEFAULT NET_30     | NET_30, NET_60, IMMEDIATE |
| isActive     | Boolean       | DEFAULT true       | Active status             |
| createdAt    | Timestamp     | NOT NULL           | Creation time             |
| updatedAt    | Timestamp     | NOT NULL           | Last update               |

---

## SalesOrder

| Field       | Type          | Constraints        | Description                                    |
| ----------- | ------------- | ------------------ | ---------------------------------------------- |
| id          | Long          | PK, Auto-increment | Primary key                                    |
| orderNumber | String(50)    | NOT NULL, UNIQUE   | Order number                                   |
| customerId  | Long          | FK → Customer      | Customer                                       |
| orderDate   | Timestamp     | NOT NULL           | Order date                                     |
| status      | Enum          | DEFAULT DRAFT      | DRAFT, CONFIRMED, SHIPPED, INVOICED, CANCELLED |
| subtotal    | Decimal(15,2) | NOT NULL           | Subtotal                                       |
| taxAmount   | Decimal(15,2) | DEFAULT 0          | Tax amount                                     |
| totalAmount | Decimal(15,2) | NOT NULL           | Total                                          |
| notes       | Text          | NULL               | Notes                                          |
| createdBy   | Long          | FK → User          | Creator                                        |
| createdAt   | Timestamp     | NOT NULL           | Creation time                                  |
| updatedAt   | Timestamp     | NOT NULL           | Last update                                    |

---

## SalesOrderLine

| Field     | Type          | Constraints        | Description |
| --------- | ------------- | ------------------ | ----------- |
| id        | Long          | PK, Auto-increment | Primary key |
| orderId   | Long          | FK → SalesOrder    | Order       |
| productId | Long          | FK → Product       | Product     |
| quantity  | Integer       | NOT NULL           | Quantity    |
| unitPrice | Decimal(15,2) | NOT NULL           | Unit price  |
| lineTotal | Decimal(15,2) | NOT NULL           | Line total  |

---

## Invoice

| Field         | Type          | Constraints           | Description                           |
| ------------- | ------------- | --------------------- | ------------------------------------- |
| id            | Long          | PK, Auto-increment    | Primary key                           |
| invoiceNumber | String(50)    | NOT NULL, UNIQUE      | Invoice number                        |
| salesOrderId  | Long          | FK → SalesOrder, NULL | Source order                          |
| customerId    | Long          | FK → Customer         | Customer                              |
| invoiceDate   | Timestamp     | NOT NULL              | Invoice date                          |
| dueDate       | Timestamp     | NOT NULL              | Due date                              |
| status        | Enum          | DEFAULT DRAFT         | DRAFT, SENT, PAID, OVERDUE, CANCELLED |
| subtotal      | Decimal(15,2) | NOT NULL              | Subtotal                              |
| taxAmount     | Decimal(15,2) | DEFAULT 0             | Tax                                   |
| total         | Decimal(15,2) | NOT NULL              | Total                                 |
| paidAmount    | Decimal(15,2) | DEFAULT 0             | Paid amount                           |
| createdAt     | Timestamp     | NOT NULL              | Creation time                         |
| updatedAt     | Timestamp     | NOT NULL              | Last update                           |

---

## Payment

| Field       | Type          | Constraints        | Description                       |
| ----------- | ------------- | ------------------ | --------------------------------- |
| id          | Long          | PK, Auto-increment | Primary key                       |
| invoiceId   | Long          | FK → Invoice       | Invoice                           |
| amount      | Decimal(15,2) | NOT NULL           | Payment amount                    |
| paymentDate | Timestamp     | NOT NULL           | Payment date                      |
| method      | Enum          | NOT NULL           | CASH, CARD, BANK_TRANSFER, CHEQUE |
| reference   | String(100)   | NULL               | Reference number                  |
| notes       | Text          | NULL               | Notes                             |
| createdAt   | Timestamp     | NOT NULL           | Creation time                     |

---

## PurchaseOrder

| Field        | Type          | Constraints        | Description                               |
| ------------ | ------------- | ------------------ | ----------------------------------------- |
| id           | Long          | PK, Auto-increment | Primary key                               |
| poNumber     | String(50)    | NOT NULL, UNIQUE   | PO number                                 |
| supplierId   | Long          | FK → Supplier      | Supplier                                  |
| orderDate    | Timestamp     | NOT NULL           | Order date                                |
| expectedDate | Date          | NULL               | Expected delivery                         |
| status       | Enum          | DEFAULT DRAFT      | DRAFT, SENT, RECEIVED, PARTIAL, CANCELLED |
| subtotal     | Decimal(15,2) | NOT NULL           | Subtotal                                  |
| totalAmount  | Decimal(15,2) | NOT NULL           | Total                                     |
| notes        | Text          | NULL               | Notes                                     |
| createdBy    | Long          | FK → User          | Creator                                   |
| createdAt    | Timestamp     | NOT NULL           | Creation time                             |
| updatedAt    | Timestamp     | NOT NULL           | Last update                               |

---

## PurchaseOrderLine

| Field       | Type          | Constraints        | Description  |
| ----------- | ------------- | ------------------ | ------------ |
| id          | Long          | PK, Auto-increment | Primary key  |
| orderId     | Long          | FK → PurchaseOrder | Order        |
| productId   | Long          | FK → Product       | Product      |
| quantity    | Integer       | NOT NULL           | Ordered qty  |
| receivedQty | Integer       | DEFAULT 0          | Received qty |
| unitPrice   | Decimal(15,2) | NOT NULL           | Unit price   |
| lineTotal   | Decimal(15,2) | NOT NULL           | Line total   |

---

## Account

| Field     | Type          | Constraints        | Description                               |
| --------- | ------------- | ------------------ | ----------------------------------------- |
| id        | Long          | PK, Auto-increment | Primary key                               |
| code      | String(50)    | NOT NULL, UNIQUE   | Account code                              |
| name      | String(255)   | NOT NULL           | Account name                              |
| type      | Enum          | NOT NULL           | ASSET, LIABILITY, EQUITY, INCOME, EXPENSE |
| parentId  | Long          | FK → Account, NULL | Parent account                            |
| balance   | Decimal(15,2) | DEFAULT 0          | Current balance                           |
| isActive  | Boolean       | DEFAULT true       | Active status                             |
| createdAt | Timestamp     | NOT NULL           | Creation time                             |
| updatedAt | Timestamp     | NOT NULL           | Last update                               |

---

## JournalEntry

| Field       | Type        | Constraints        | Description   |
| ----------- | ----------- | ------------------ | ------------- |
| id          | Long        | PK, Auto-increment | Primary key   |
| entryNumber | String(50)  | NOT NULL, UNIQUE   | Entry number  |
| date        | Date        | NOT NULL           | Entry date    |
| description | String(500) | NOT NULL           | Description   |
| reference   | String(100) | NULL               | Reference     |
| status      | Enum        | DEFAULT DRAFT      | DRAFT, POSTED |
| createdBy   | Long        | FK → User          | Creator       |
| createdAt   | Timestamp   | NOT NULL           | Creation time |
| postedAt    | Timestamp   | NULL               | Posted time   |

---

## JournalEntryLine

| Field       | Type          | Constraints        | Description      |
| ----------- | ------------- | ------------------ | ---------------- |
| id          | Long          | PK, Auto-increment | Primary key      |
| entryId     | Long          | FK → JournalEntry  | Entry            |
| accountId   | Long          | FK → Account       | Account          |
| debit       | Decimal(15,2) | DEFAULT 0          | Debit amount     |
| credit      | Decimal(15,2) | DEFAULT 0          | Credit amount    |
| description | String(255)   | NULL               | Line description |

---

## Attendance

| Field      | Type      | Constraints        | Description                  |
| ---------- | --------- | ------------------ | ---------------------------- |
| id         | Long      | PK, Auto-increment | Primary key                  |
| employeeId | Long      | FK → Employee      | Employee                     |
| date       | Date      | NOT NULL           | Attendance date              |
| checkIn    | Timestamp | NULL               | Clock in time                |
| checkOut   | Timestamp | NULL               | Clock out time               |
| status     | Enum      | DEFAULT PRESENT    | PRESENT, ABSENT, LATE, LEAVE |
| notes      | Text      | NULL               | Notes                        |

---

## LeaveRequest

| Field      | Type            | Constraints        | Description                            |
| ---------- | --------------- | ------------------ | -------------------------------------- |
| id         | Long            | PK, Auto-increment | Primary key                            |
| employeeId | Long            | FK → Employee      | Employee                               |
| startDate  | Date            | NOT NULL           | Start date                             |
| endDate    | Date            | NOT NULL           | End date                               |
| type       | Enum            | NOT NULL           | ANNUAL, SICK, PERSONAL, UNPAID         |
| status     | Enum            | DEFAULT PENDING    | PENDING, APPROVED, REJECTED, CANCELLED |
| reason     | Text            | NOT NULL           | Reason                                 |
| approvedBy | Long            | FK → User, NULL    | Approver                               |
| approvedAt | Timestamp, NULL | NULL               | Approval time                          |
| createdAt  | Timestamp       | NOT NULL           | Creation time                          |
| updatedAt  | Timestamp       | NOT NULL           | Last update                            |

---

## LeaveBalance

| Field      | Type    | Constraints        | Description            |
| ---------- | ------- | ------------------ | ---------------------- |
| id         | Long    | PK, Auto-increment | Primary key            |
| employeeId | Long    | FK → Employee      | Employee               |
| type       | Enum    | NOT NULL           | ANNUAL, SICK, PERSONAL |
| totalDays  | Integer | NOT NULL           | Total days             |
| usedDays   | Integer | DEFAULT 0          | Used days              |

---

## AuditLog

| Field      | Type        | Constraints        | Description    |
| ---------- | ----------- | ------------------ | -------------- |
| id         | Long        | PK, Auto-increment | Primary key    |
| userId     | Long        | FK → User          | User           |
| action     | String(100) | NOT NULL           | Action         |
| entityType | String(100) | NOT NULL           | Entity type    |
| entityId   | Long        | NOT NULL           | Entity ID      |
| changes    | JSON        | NULL               | Changed fields |
| ipAddress  | String(45)  | NULL               | IP address     |
| createdAt  | Timestamp   | NOT NULL           | Creation time  |

---

## Enums

### OrderStatus

```
DRAFT, CONFIRMED, SHIPPED, INVOICED, CANCELLED
```

### InvoiceStatus

```
DRAFT, SENT, PAID, OVERDUE, CANCELLED
```

### PurchaseOrderStatus

```
DRAFT, SENT, RECEIVED, PARTIAL, CANCELLED
```

### AccountType

```
ASSET, LIABILITY, EQUITY, INCOME, EXPENSE
```

### EmployeeStatus

```
ACTIVE, INACTIVE, TERMINATED
```

### LeaveType

```
ANNUAL, SICK, PERSONAL, UNPAID
```

### LeaveStatus

```
PENDING, APPROVED, REJECTED, CANCELLED
```

### AttendanceStatus

```
PRESENT, ABSENT, LATE, LEAVE
```

### StockMovementType

```
IN, OUT, ADJUSTMENT
```

### PaymentMethod

```
CASH, CARD, BANK_TRANSFER, CHEQUE
```
