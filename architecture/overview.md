# System Architecture Overview

## Introduction

The ERP system is a full-stack web application designed for small-to-medium enterprises to manage core business operations including inventory, sales, purchasing, finance, and human resources.

---

## High-Level Architecture

The system follows a standard client-server architecture:

- **Client**: React + TypeScript SPA running in the browser
- **Load Balancer / API Gateway**: Routes traffic, handles SSL termination
- **Frontend Server**: Serves static assets and handles SPA routing
- **Backend API**: Spring Boot REST API handling business logic
- **PostgreSQL**: Primary database for all business data
- **Redis**: Cache and session management

---

## Technology Stack

### Frontend

| Layer | Technology | Purpose |
|-------|------------|---------|
| Framework | React 18+ | UI library |
| Language | TypeScript | Type safety |
| State | Redux Toolkit / Context API | Global state management |
| Routing | React Router v6 | Client-side routing |
| HTTP Client | Axios | API communication |
| UI Library | Ant Design / Material-UI | Component library |
| Charts | Recharts / Chart.js | Data visualization |
| Forms | React Hook Form + Zod | Form handling & validation |
| Testing | Jest + React Testing Library | Unit & integration tests |

### Backend

| Layer | Technology | Purpose |
|-------|------------|---------|
| Framework | Spring Boot 3.x | REST API framework |
| Language | Java 17+ | Backend language |
| ORM | Spring Data JPA | Database abstraction |
| Database | PostgreSQL | Primary data store |
| Cache | Redis | Session & data caching |
| Security | Spring Security + JWT | Authentication & authorization |
| Validation | Hibernate Validator | Request validation |
| Docs | SpringDoc OpenAPI | API documentation |
| Testing | JUnit 5 + Mockito | Unit & integration tests |

### DevOps

| Tool | Purpose |
|------|---------|
| Docker | Containerization |
| Docker Compose | Local development |
| GitHub Actions | CI/CD pipeline |
| Flyway | Database migrations |

---

## Module Architecture

The system is organized into **7 main modules**, each with frontend pages and backend APIs:

### Module Overview

| Module | Frontend | Backend | Primary Entities |
|--------|----------|---------|------------------|
| **Dashboard** | Widgets, charts | Aggregated stats | - |
| **Inventory** | Products, categories | Product, Category, StockMovement | [Class Diagram](diagrams/class/inventory.md) |
| **Sales** | Orders, customers | SalesOrder, Customer, Invoice | [Class Diagram](diagrams/class/sales.md) |
| **Purchasing** | POs, suppliers | PurchaseOrder, Supplier | [Class Diagram](diagrams/class/purchasing.md) |
| **Finance** | Invoices, accounts | Invoice, Payment, Account, JournalEntry | [Class Diagram](diagrams/class/finance.md) |
| **HR** | Employees, attendance | Employee, Attendance, LeaveRequest | [Class Diagram](diagrams/class/hr.md) |
| **Admin** | Users, settings | User, Role, AuditLog | [Class Diagram](diagrams/class/auth.md) |

---

## System Components

### Frontend Components

The frontend uses a shell layout with:
- **Sidebar**: Navigation menu with collapsible module sections
- **Header**: Page title, breadcrumbs, user menu
- **Content Area**: Dynamic page content

### Backend Components

The backend follows a layered architecture:
- **Controller Layer**: REST endpoints, request handling
- **Service Layer**: Business logic, transactions
- **Repository Layer**: Data access, JPA queries
- **Entity Layer**: JPA entities, database mapping

---

## Key Design Patterns

### Backend Patterns

| Pattern | Usage |
|---------|-------|
| **Layered Architecture** | Controller → Service → Repository |
| **DTO Pattern** | Separate API contracts from entities |
| **Mapper Pattern** | Entity ↔ DTO conversion |
| **Repository Pattern** | Abstract database access |
| **Factory Pattern** | Complex object creation |
| **Strategy Pattern** | Multiple validation/shipping algorithms |

### Frontend Patterns

| Pattern | Usage |
|---------|-------|
| **Container/Presentational** | Logic vs display separation |
| **Custom Hooks** | Reusable logic extraction |
| **Context API** | Global state distribution |
| **Service Layer** | Centralized API calls |
| **Error Boundaries** | Graceful error handling |

---

## Data Flow

### Request Lifecycle

1. User triggers an action (click, form submit)
2. React component dispatches action
3. Service layer sends HTTP request with JWT token
4. Backend receives request via security filter
5. Security filter validates JWT
6. Controller routes to appropriate handler
7. Service processes business logic
8. Repository accesses database
9. Response travels back up the stack
10. React updates state and UI re-renders

---

## Security Architecture

Security is implemented in layers:

| Layer | Protection |
|-------|------------|
| 1. Network | HTTPS, TLS 1.3, Load Balancer SSL offload |
| 2. API Gateway | Rate limiting, IP allowlist |
| 3. Authentication | JWT with access/refresh tokens |
| 4. Authorization | Role-based (RBAC) |
| 5. Data | Input validation, SQL injection prevention |
| 6. Audit | All actions logged |

---

## Architecture Diagrams

### Required Diagrams (by team pair)

| Diagram Type | File | Owner |
|--------------|------|-------|
| **Use Case** | | |
| Master Use Case | `diagrams/usecase/system-overview.md` | Pair C |
| Inventory Use Cases | `diagrams/usecase/inventory.md` | Pair A |
| Purchasing Use Cases | `diagrams/usecase/purchasing.md` | Pair A |
| Sales Use Cases | `diagrams/usecase/sales.md` | Pair B |
| Finance Use Cases | `diagrams/usecase/finance.md` | Pair B |
| Auth Use Cases | `diagrams/usecase/auth.md` | Pair C |
| HR Use Cases | `diagrams/usecase/hr.md` | Pair C |
| **Sequence** | | |
| System Flows | `diagrams/sequence/system-flows.md` | Pair B |
| Stock Updates | `diagrams/sequence/stock-updates.md` | Pair A |
| Sales Flow | `diagrams/sequence/sales-flow.md` | Pair B |
| Payment | `diagrams/sequence/payment.md` | Pair B |
| Authentication | `diagrams/sequence/auth.md` | Pair C |
| Leave Approval | `diagrams/sequence/leave-approval.md` | Pair C |
| **Class** | | |
| Entity Relationship | `diagrams/class/entity-relationship.md` | BE1 |
| Inventory Entities | `diagrams/class/inventory.md` | Pair A |
| Purchasing Entities | `diagrams/class/purchasing.md` | Pair A |
| Sales Entities | `diagrams/class/sales.md` | Pair B |
| Finance Entities | `diagrams/class/finance.md` | Pair B |
| Auth Entities | `diagrams/class/auth.md` | Pair C |
| HR Entities | `diagrams/class/hr.md` | Pair C |
| **Component** | | |
| Product Pages | `diagrams/component/product-pages.md` | Pair A |
| Purchasing Pages | `diagrams/component/purchasing-pages.md` | Pair A |
| Sales Pages | `diagrams/component/sales-pages.md` | Pair B |
| Finance Pages | `diagrams/component/finance-pages.md` | Pair B |
| Dashboard | `diagrams/component/dashboard.md` | Pair C |
| Admin Pages | `diagrams/component/admin-pages.md` | Pair C |
| HR Pages | `diagrams/component/hr-pages.md` | Pair C |
| **Activity** | | |
| Workflows | `diagrams/activity/workflows.md` | Pair A |

---

## Non-Functional Requirements

### Performance
- API response time: < 200ms for simple queries
- Page load time: < 3 seconds
- Support 100 concurrent users

### Scalability
- Horizontal scaling via stateless design
- Database connection pooling
- Redis caching layer

### Security
- JWT token expiration: 15 minutes (access), 7 days (refresh)
- Password hashing: BCrypt
- HTTPS only
- CSRF protection

### Reliability
- 99.9% uptime target
- Graceful degradation
- Comprehensive error handling

### Maintainability
- Clean code principles
- Comprehensive documentation
- Automated testing (unit + integration)

---

## Development Phases

| Phase | Focus | Duration |
|-------|-------|---------|
| 1 - MVP | Core transactions (Inventory, Sales basics) | 4 weeks |
| 2 - Expand | Purchasing, Finance, Reporting | 4 weeks |
| 3 - HR & Admin | Employees, Attendance, User Management | 3 weeks |
| 4 - Polish | Integration, Notifications, Optimization | 3 weeks |

---

## Related Documentation

- [Navigation Structure](navigation.md)
- [Pages Inventory](pages.md)
- [API Overview](../api/overview.md)
- [Frontend Guidelines](../guidelines/frontend.md)
- [Backend Guidelines](../guidelines/backend.md)
- [Team Roles & Diagram Assignment](../team/roles.md)
