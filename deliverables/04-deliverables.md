# Deliverables 04 - Development Implementation

## Overview

This phase focuses on parallel development: backend sets up infrastructure while frontend connects pages to APIs.

---

## Phase Goals

| Goal        | Description                      |
| ----------- | -------------------------------- |
| Backend     | Docker + Database + Working APIs |
| Frontend    | API client + Connected pages     |
| Integration | End-to-end CRUD operations       |

---

## Backend Deliverables

### Infrastructure Setup

| Task            | Owner   | Description                | Status  |
| --------------- | ------- | -------------------------- | ------- |
| Docker Compose  | BE team | PostgreSQL + Redis         | Pending |
| Database schema | BE1     | Create tables/migrations   | Pending |
| Seed data       | BE team | Products, customers, users | Pending |

### API Endpoints to Verify

| Module       | Endpoints                                      | Status  |
| ------------ | ---------------------------------------------- | ------- |
| Auth         | `/auth/login`, `/auth/refresh`, `/auth/logout` | Pending |
| Users        | `/users/*`                                     | Pending |
| Products     | `/products/*`                                  | Pending |
| Categories   | `/categories/*`                                | Pending |
| Suppliers    | `/suppliers/*`                                 | Pending |
| Customers    | `/customers/*`                                 | Pending |
| Sales Orders | `/sales-orders/*`                              | Pending |
| Invoices     | `/invoices/*`                                  | Pending |
| Employees    | `/employees/*`                                 | Pending |
| Attendance   | `/attendance/*`                                | Pending |
| Leave        | `/leave-requests/*`                            | Pending |

---

## Frontend Deliverables

### Shared Infrastructure

| Task         | Owner  | Description                      | Status  |
| ------------ | ------ | -------------------------------- | ------- |
| API Client   | Pair A | Axios instance with interceptors | Pending |
| Auth Context | Pair C | Token storage, login/logout      | Pending |
| API Types    | All    | TypeScript types matching DTOs   | Pending |

### Page Connections by Pair

#### Pair A (FE1) - Inventory & Purchasing

| Page             | API Endpoint           | Status  |
| ---------------- | ---------------------- | ------- |
| Product List     | `GET /products`        | Pending |
| Add Product      | `POST /products`       | Pending |
| Product Details  | `GET /products/:id`    | Pending |
| Edit Product     | `PUT /products/:id`    | Pending |
| Categories       | `GET/POST /categories` | Pending |
| Suppliers List   | `GET /suppliers`       | Pending |
| Supplier Details | `GET /suppliers/:id`   | Pending |
| Purchase Orders  | `GET /purchase-orders` | Pending |

#### Pair B (FE2) - Sales & Finance

| Page              | API Endpoint                | Status  |
| ----------------- | --------------------------- | ------- |
| Customers List    | `GET /customers`            | Pending |
| Customer Details  | `GET /customers/:id`        | Pending |
| Sales Orders List | `GET /sales-orders`         | Pending |
| Sales Order Form  | `POST /sales-orders`        | Pending |
| Invoices List     | `GET /invoices`             | Pending |
| Invoice Details   | `GET /invoices/:id`         | Pending |
| Invoice Form      | `POST /invoices`            | Pending |
| Journal Entries   | `GET/POST /journal-entries` | Pending |
| Chart of Accounts | `GET /accounts`             | Pending |

#### Pair C (FE3) - HR, Admin & Dashboard

| Page             | API Endpoint               | Status  |
| ---------------- | -------------------------- | ------- |
| Login            | `POST /auth/login`         | Pending |
| Dashboard        | `GET /dashboard/stats`     | Pending |
| Users List       | `GET /users`               | Pending |
| Employees List   | `GET /employees`           | Pending |
| Employee Details | `GET /employees/:id`       | Pending |
| Attendance       | `GET /attendance`          | Pending |
| Leave Requests   | `GET/POST /leave-requests` | Pending |
| Settings         | `GET/PUT /settings`        | Pending |
| Audit Logs       | `GET /audit-logs`          | Pending |

---

## Technical Setup

### Vite Proxy Configuration

Add to `web/vite.config.ts`:

```typescript
server: {
  proxy: {
    '/api': 'http://localhost:8080'
  }
}
```

### API Client Pattern

```typescript
// src/services/apiClient.ts
import axios from "axios";

const api = axios.create({
  baseURL: "/api/v1",
});

// Add auth interceptor
api.interceptors.request.use((config) => {
  const token = localStorage.getItem("token");
  if (token) {
    config.headers.Authorization = `Bearer ${token}`;
  }
  return config;
});

export default api;
```

---

## Development Order

### Week 1: Infrastructure

| Backend             | Frontend     |
| ------------------- | ------------ |
| Docker + DB setup   | API client   |
| Test auth endpoints | Auth context |
| Seed data           | Login page   |

### Week 2: Core Connections

| Backend               | Frontend         |
| --------------------- | ---------------- |
| Product/Category APIs | Inventory pages  |
| Supplier APIs         | Purchasing pages |
| Customer APIs         | Sales pages      |

### Week 3: Module Expansion

| Backend               | Frontend           |
| --------------------- | ------------------ |
| Invoice/Payment APIs  | Finance pages      |
| Employee APIs         | HR pages           |
| Dashboard aggregation | Dashboard + Charts |

### Week 4: Integration

| Task               | Owner |
| ------------------ | ----- |
| End-to-end testing | All   |
| Error handling     | All   |
| Loading states     | All   |

---

## Previous Deliverables

- [01-deliverables.md](./01-deliverables.md) - UML diagrams
- [02-deliverables.md](./02-deliverables.md) - Shared system diagrams
- [03-deliverables.md](./03-deliverables.md) - Implementation plan

---

## Status

| Phase | Description            | Status         |
| ----- | ---------------------- | -------------- |
| 04a   | Backend infrastructure | 🚧 In Progress |
| 04b   | Frontend API client    | 🚧 In Progress |
| 04c   | Page connections       | ⏳ Pending     |
| 04d   | Integration testing    | ⏳ Pending     |
