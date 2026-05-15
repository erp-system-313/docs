# ERP System — Documentation

## Structure

```
docs/
├── README.md                      ← This file
├── project-overview.md            ← High-level project summary
├── requirements.md                ← Comprehensive requirements document
│
├── api/                           ← API specifications
│   ├── data-models.md             ← All database entities, fields, enums
│   └── endpoints.md               ← All REST API endpoints with examples
│
├── architecture/                  ← System architecture
│   ├── overview.md                ← High-level architecture, tech stack, patterns
│   ├── navigation.md              ← Sidebar navigation structure, routes
│   ├── pages.md                   ← All frontend pages by module
│   └── diagrams/                  ← UML diagrams (PNG)
│       ├── usecase/               ← Use case diagrams
│       ├── sequence/              ← Sequence diagrams
│       ├── class/                 ← Class diagrams
│       ├── component/             ← FE component diagrams (wireframes)
│       ├── activity/              ← Activity diagrams
│       └── erd/                   ← Entity Relationship Diagrams
│
├── guidelines/                    ← Development conventions
│   ├── backend.md                 ← Java/Spring Boot conventions
│   ├── frontend.md                ← React/TypeScript conventions
│   ├── git.md                     ← Git workflow, commit format, PR process
│   └── linting.md                 ← ESLint, Prettier, Checkstyle config
│
├── setup/                         ← Environment setup
│   └── folder-structure.md        ← Project directory layout
│
├── development/                   ← Dev guides
│   └── backend-setup.md           ← Backend development environment setup
│
├── team/                          ← Team info
│   └── members.md                 ← Team member names and IDs
│
├── deliverables/                  ← Phase deliverables (01-05)
│   └── 01- through 05-deliverables.md
│
└── meetings/                      ← Meeting notes (01-05)
    └── 01- through 05-meeting.md
```

## Key Documents

| Document                                                   | Description                                                                                                     |
| ---------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------- |
| [requirements.md](./requirements.md)                       | Comprehensive spec covering all 10+ modules, architecture, API, DB, frontend, security, testing, and dev phases |
| [api/endpoints.md](./api/endpoints.md)                     | All REST API endpoints with request/response examples                                                           |
| [api/data-models.md](./api/data-models.md)                 | All database entities, fields, types, constraints, and enums                                                    |
| [architecture/overview.md](./architecture/overview.md)     | System architecture, tech stack, design patterns, security, NFRs                                                |
| [architecture/pages.md](./architecture/pages.md)           | All 39 frontend pages across 10 modules                                                                         |
| [architecture/navigation.md](./architecture/navigation.md) | Sidebar navigation structure and URL conventions                                                                |
| [guidelines/backend.md](./guidelines/backend.md)           | Java/Spring Boot coding conventions and examples                                                                |
| [guidelines/frontend.md](./guidelines/frontend.md)         | React/TypeScript coding conventions and examples                                                                |

## Tech Stack

- **Frontend:** React 19 + TypeScript + Vite + Ant Design 6
- **Backend:** Spring Boot 3.x + Java 21 + Maven + PostgreSQL 15 + Redis 7
- **DevOps:** Docker, Docker Compose, Nix Flakes, Flyway

## Current Status

| Module             | Backend | Frontend | Status                               |
| ------------------ | ------- | -------- | ------------------------------------ |
| Auth               | ✅      | ✅       | Complete                             |
| Dashboard          | ✅      | ✅       | Complete                             |
| Admin              | ✅      | ✅       | Complete                             |
| HR                 | ✅      | ✅       | Complete                             |
| Inventory          | ✅      | ✅       | Complete                             |
| Sales              | ✅      | ✅       | Complete                             |
| Purchasing         | ✅      | ✅       | Complete                             |
| Finance            | ✅      | ✅       | Complete                             |
| Project Management | ✅      | ✅       | Complete                             |
| CRM                | ❌      | ✅       | FE done (no sidebar), BE not started |
| Helpdesk           | ✅      | ✅       | Complete                             |
