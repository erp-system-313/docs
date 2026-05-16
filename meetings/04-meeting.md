# Meeting 04 - Development Kickoff

**Date:** April 16, 2026
**Attendees:** Development team (6 members)

---

## Summary

Kicked off parallel development. Backend sets up infrastructure while frontend connects pages to APIs.

---

## Repository Status

| Repo       | Pages    | Controllers    | Status            |
| ---------- | -------- | -------------- | ----------------- |
| `web/`     | 30 pages | -              | ⚠️ 4 TS errors    |
| `service/` | -        | 17 controllers | ⚠️ DB not running |

---

## Decisions Made

| Decision        | Value                                            |
| --------------- | ------------------------------------------------ |
| API Base URL    | `/api/v1` via Vite proxy                         |
| API Calls       | `/api/v1/products` → proxies to `localhost:8080` |
| Mock Data       | Yes - seed DB with products, customers, users    |
| Page Connection | Each pair connects their own module's pages      |

---

## Parallel Development

### Backend Team

| Task            | Owner   | Description                |
| --------------- | ------- | -------------------------- |
| Docker setup    | BE team | `docker-compose up -d`     |
| Database schema | BE1     | Create migrations          |
| Seed data       | BE team | Products, customers, users |
| Test endpoints  | All     | Verify APIs work           |

### Frontend Team

| Task                   | Owner  | Module                |
| ---------------------- | ------ | --------------------- |
| API client             | Pair A | Axios + interceptors  |
| Auth service           | Pair C | Login → `/auth/login` |
| Product service        | Pair A | Inventory pages       |
| Customer service       | Pair B | Sales pages           |
| Order/Invoice services | Pair B | Finance pages         |
| Employee/HR services   | Pair C | HR pages              |

---

## Coordination

| When    | Backend     | Frontend            |
| ------- | ----------- | ------------------- |
| Day 1   | Docker + DB | API client          |
| Day 2-3 | Seed data   | Connect services    |
| Ongoing | Test APIs   | Integration testing |

---

## Next Steps

1. Backend: Start Docker + create DB schema
2. Frontend: Set up API client with Vite proxy
3. Both: Connect pages to actual endpoints
