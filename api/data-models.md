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

---

## Project

| Field      | Type          | Constraints         | Description                                     |
| ---------- | ------------- | ------------------- | ----------------------------------------------- |
| id         | Long          | PK, Auto-increment  | Primary key                                     |
| name       | String(255)   | NOT NULL            | Project name                                    |
| customerId | Long          | FK → Customer, NULL | Customer                                        |
| dateStart  | Date          | NULL                | Start date                                      |
| dateEnd    | Date          | NULL                | End date                                        |
| budget     | Decimal(15,2) | NULL                | Budget                                          |
| state      | Enum          | DEFAULT PLANNING    | PLANNING, ACTIVE, ON_HOLD, COMPLETED, CANCELLED |
| createdAt  | Timestamp     | NOT NULL            | Creation time                                   |
| updatedAt  | Timestamp     | NULL                | Last update                                     |

---

## Task

| Field          | Type         | Constraints            | Description     |
| -------------- | ------------ | ---------------------- | --------------- |
| id             | Long         | PK, Auto-increment     | Primary key     |
| projectId      | Long         | FK → Project, NOT NULL | Project         |
| name           | String(255)  | NOT NULL               | Task name       |
| description    | Text         | NULL                   | Description     |
| assignedTo     | Long         | FK → Employee, NULL    | Assignee        |
| stageId        | Long         | FK → TaskStage, NULL   | Stage           |
| dueDate        | Date         | NULL                   | Due date        |
| estimatedHours | Decimal(8,2) | NULL                   | Estimated hours |
| actualHours    | Decimal(8,2) | NULL                   | Actual hours    |
| createdAt      | Timestamp    | NOT NULL               | Creation time   |
| updatedAt      | Timestamp    | NULL                   | Last update     |

---

## TaskStage

| Field     | Type        | Constraints            | Description   |
| --------- | ----------- | ---------------------- | ------------- |
| id        | Long        | PK, Auto-increment     | Primary key   |
| projectId | Long        | FK → Project, NOT NULL | Project       |
| name      | String(100) | NOT NULL               | Stage name    |
| sequence  | Integer     | DEFAULT 0              | Display order |
| isDefault | Boolean     | DEFAULT false          | Default stage |

---

## Lead (Planned — CRM)

| Field      | Type        | Constraints        | Description                                |
| ---------- | ----------- | ------------------ | ------------------------------------------ |
| id         | Long        | PK, Auto-increment | Primary key                                |
| name       | String(255) | NOT NULL           | Contact name                               |
| email      | String(255) | NULL               | Email                                      |
| phone      | String(20)  | NULL               | Phone                                      |
| company    | String(255) | NULL               | Company name                               |
| source     | String(50)  | NULL               | WEBSITE, REFERRAL, COLD_CALL, etc          |
| status     | Enum        | DEFAULT NEW        | NEW, CONTACTED, QUALIFIED, CONVERTED, LOST |
| assignedTo | Long        | FK → User, NULL    | Assigned user                              |
| notes      | Text        | NULL               | Notes                                      |
| createdAt  | Timestamp   | NOT NULL           | Creation time                              |
| updatedAt  | Timestamp   | NOT NULL           | Last update                                |

---

## Opportunity (Planned — CRM)

| Field       | Type          | Constraints                  | Description       |
| ----------- | ------------- | ---------------------------- | ----------------- |
| id          | Long          | PK, Auto-increment           | Primary key       |
| leadId      | Long          | FK → Lead, NULL              | Source lead       |
| customerId  | Long          | FK → Customer, NOT NULL      | Customer          |
| stageId     | Long          | FK → PipelineStage, NOT NULL | Pipeline stage    |
| revenue     | Decimal(15,2) | DEFAULT 0                    | Expected revenue  |
| probability | Integer       | DEFAULT 0                    | Win probability % |
| closeDate   | Date          | NULL                         | Expected close    |
| createdAt   | Timestamp     | NOT NULL                     | Creation time     |
| updatedAt   | Timestamp     | NOT NULL                     | Last update       |

---

## PipelineStage (Planned — CRM)

| Field     | Type        | Constraints        | Description   |
| --------- | ----------- | ------------------ | ------------- |
| id        | Long        | PK, Auto-increment | Primary key   |
| name      | String(100) | NOT NULL           | Stage name    |
| sequence  | Integer     | NOT NULL           | Display order |
| createdAt | Timestamp   | NOT NULL           | Creation time |

---

## Ticket (Helpdesk)

| Field            | Type               | Constraints         | Description                                    |
| ---------------- | ------------------ | ------------------- | ---------------------------------------------- |
| id               | Long               | PK, Auto-increment  | Primary key                                    |
| title            | String(255)        | NOT NULL            | Ticket title                                   |
| description      | Text               | NULL                | Issue description                              |
| customer         | Customer           | @ManyToOne(LAZY)    | FK customer_id → customers(id)                 |
| priority         | Enum               | NOT NULL            | LOW, MEDIUM, HIGH, URGENT                      |
| status           | Enum               | NOT NULL            | OPEN, IN_PROGRESS, RESOLVED, CLOSED            |
| assignedTo       | Employee           | @ManyToOne(LAZY)    | FK assigned_to → employees(id), NULL           |
| createdBy        | User               | @ManyToOne(LAZY)    | FK created_by → users(id), NULL                |
| stage            | HelpdeskStage      | @ManyToOne(LAZY)    | FK stage_id → helpdesk_stages(id), NULL        |
| team             | HelpdeskTeam       | @ManyToOne(LAZY)    | FK team_id → helpdesk_teams(id), NULL          |
| category         | HelpdeskCategory   | @ManyToOne(LAZY)    | FK category_id → helpdesk_categories(id), NULL |
| channel          | Enum               | NULL                | EMAIL, PHONE, CHAT, PORTAL                     |
| slaDeadline      | Timestamp          | NULL                | SLA resolution deadline                        |
| slaStatus        | Enum               | NULL                | OK, WARNING, BREACHED                          |
| closedAt         | Timestamp          | NULL                | Time ticket was closed                         |
| isArchived       | Boolean            | DEFAULT false       | Archived flag                                  |
| tags             | Set<HelpdeskTag>   | @ManyToMany         | Ticket tags                                    |
| ticketCommentSet | Set<TicketComment> | @OneToMany(CASCADE) | Comments on this ticket                        |
| createdAt        | Timestamp          | NOT NULL            | Creation time                                  |
| updatedAt        | Timestamp          | NULL                | Last update                                    |

---

## TicketComment (Helpdesk)

| Field      | Type      | Constraints        | Description                          |
| ---------- | --------- | ------------------ | ------------------------------------ |
| id         | Long      | PK, Auto-increment | Primary key                          |
| ticket     | Ticket    | @ManyToOne(LAZY)   | FK ticket_id → tickets(id), NOT NULL |
| author     | User      | @ManyToOne(LAZY)   | FK author_id → users(id), NOT NULL   |
| message    | Text      | NOT NULL           | Comment body                         |
| isInternal | Boolean   | DEFAULT false      | Internal note                        |
| createdAt  | Timestamp | NOT NULL           | Creation time                        |

> **Note:** Comments can be created via `POST /support/tickets/{id}/comments` and are also loaded via `Ticket.@OneToMany`.

---

## HelpdeskTeam

| Field     | Type        | Constraints         | Description   |
| --------- | ----------- | ------------------- | ------------- |
| id        | Long        | PK, Auto-increment  | Primary key   |
| name      | String(100) | NOT NULL            | Team name     |
| leadId    | Long        | FK → Employee, NULL | Team lead     |
| isActive  | Boolean     | DEFAULT true        | Active status |
| createdAt | Timestamp   | NOT NULL            | Creation time |

---

## HelpdeskStage

| Field     | Type        | Constraints        | Description   |
| --------- | ----------- | ------------------ | ------------- |
| id        | Long        | PK, Auto-increment | Primary key   |
| name      | String(100) | NOT NULL           | Stage name    |
| sequence  | Integer     | DEFAULT 0          | Display order |
| isDefault | Boolean     | DEFAULT false      | Default stage |

---

## HelpdeskCategory

| Field       | Type        | Constraints                 | Description     |
| ----------- | ----------- | --------------------------- | --------------- |
| id          | Long        | PK, Auto-increment          | Primary key     |
| name        | String(100) | NOT NULL                    | Category name   |
| description | Text        | NULL                        | Description     |
| parentId    | Long        | FK → HelpdeskCategory, NULL | Parent category |
| isActive    | Boolean     | DEFAULT true                | Active status   |
| createdAt   | Timestamp   | NOT NULL                    | Creation time   |

---

## HelpdeskTag

| Field | Type       | Constraints        | Description |
| ----- | ---------- | ------------------ | ----------- |
| id    | Long       | PK, Auto-increment | Primary key |
| name  | String(50) | NOT NULL, UNIQUE   | Tag name    |
| color | String(7)  | NULL               | Hex color   |

---

## SlaPolicy

| Field          | Type        | Constraints        | Description                |
| -------------- | ----------- | ------------------ | -------------------------- |
| id             | Long        | PK, Auto-increment | Primary key                |
| name           | String(100) | NOT NULL           | Policy name                |
| priority       | Enum        | NOT NULL           | LOW, MEDIUM, HIGH, URGENT  |
| responseTime   | Integer     | NOT NULL           | Response time in minutes   |
| resolutionTime | Integer     | NOT NULL           | Resolution time in minutes |
| isActive       | Boolean     | DEFAULT true       | Active status              |
| createdAt      | Timestamp   | NOT NULL           | Creation time              |

---

## KbArticle

| Field       | Type        | Constraints                 | Description     |
| ----------- | ----------- | --------------------------- | --------------- |
| id          | Long        | PK, Auto-increment          | Primary key     |
| title       | String(255) | NOT NULL                    | Article title   |
| content     | Text        | NOT NULL                    | Article body    |
| categoryId  | Long        | FK → HelpdeskCategory, NULL | Category        |
| tags        | String(500) | NULL                        | Comma-separated |
| views       | Integer     | DEFAULT 0                   | View count      |
| isPublished | Boolean     | DEFAULT false               | Published flag  |
| createdBy   | Long        | FK → User                   | Author          |
| createdAt   | Timestamp   | NOT NULL                    | Creation time   |
| updatedAt   | Timestamp   | NULL                        | Last update     |

---

## TicketAttachment

| Field      | Type        | Constraints        | Description   |
| ---------- | ----------- | ------------------ | ------------- |
| id         | Long        | PK, Auto-increment | Primary key   |
| ticketId   | Long        | FK → Ticket        | Parent ticket |
| fileName   | String(255) | NOT NULL           | Original name |
| filePath   | String(500) | NOT NULL           | Storage path  |
| mimeType   | String(100) | NULL               | MIME type     |
| fileSize   | Long        | NULL               | Size in bytes |
| uploadedBy | Long        | FK → User          | Uploader      |
| createdAt  | Timestamp   | NOT NULL           | Creation time |

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

### ProjectState

```
PLANNING, ACTIVE, ON_HOLD, COMPLETED, CANCELLED
```

### LeadStatus

```
NEW, CONTACTED, QUALIFIED, CONVERTED, LOST
```

### TicketChannel

```
EMAIL, PHONE, CHAT, PORTAL
```

### SlaStatus

```
OK, WARNING, BREACHED
```

### TicketPriority

```
LOW, MEDIUM, HIGH, URGENT
```

### TicketStatus

```
OPEN, IN_PROGRESS, RESOLVED, CLOSED
```
