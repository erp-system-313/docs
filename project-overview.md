# ERP System — Overview

A unified ERP platform to streamline daily operations, improve data visibility, and support decision-making. The system follows a modular architecture with a Spring Boot backend and React TypeScript frontend, making it easy to maintain and extend.

## Key Features

- **Dashboard**: Real-time overview of key metrics (sales, low stock, pending invoices)
- **Inventory Management**: Products, categories, stock levels, UoM, and stock movements
- **Sales & CRM**: Sales orders, customers, partners, price lists, pipeline management, lead conversion
- **Purchasing & Suppliers**: Purchase orders, supplier management, goods receiving
- **Finance & Accounting**: Chart of accounts, invoices, journal entries, payments, taxes, bank statements, analytic accounting, financial reports (balance sheet, P&L, trial balance, general ledger)
- **Human Resources**: Employee records, attendance clock-in/out, leave requests & approvals, contracts, departments, job positions
- **Administration**: Users, roles, permissions, audit logs, system settings
- **Support/Helpdesk**: Tickets, comments, teams, stages, SLA, knowledge base
- **Projects**: Projects, tasks, stages, Gantt charts
- **Recruitment**: Job openings, applicants, pipeline stages

## Tech Stack

- **Frontend**: React 19, TypeScript, Vite 8, Ant Design 6, react-hook-form + yup, dayjs, axios, Recharts
- **Backend**: Java 21, Spring Boot 3.2.5, Spring Data JPA, Spring Security + JWT, Flyway (PostgreSQL migrations), Redis
- **DevOps**: Docker, Docker Compose, Nix flake, GitHub Actions

## Team

- 3 Frontend Developers
- 3 Backend Developers

## Modules

The application consists of **10+ modules** with **55+ pages**:

| Module | Pages |
|--------|-------|
| Auth | Login |
| Common | Dashboard, Profile |
| Admin | Users, Roles, Settings, Audit Logs |
| CRM | Dashboard, Leads, Lead Details, Pipeline |
| Sales | Customers List, Customer Details, Orders List, Order Details, Order Form |
| Finance | Chart of Accounts, Invoices List, Invoice Details, Invoice Form, Journal Entries, Journal Entry Form |
| Inventory | Product List, Product Details, Create/Edit Product, Category List |
| Purchasing | Supplier List, Supplier Details, PO List, Create PO |
| HR | HR Dashboard, Employees List, Employee Details, Attendance, Leave Requests, Leave Allocations, Leave Calendar, Departments, Job Positions, Contracts |
| Support | Tickets List, Ticket Details, Create/Edit Ticket |
| Projects | Project List, Project Detail, Gantt |
| Recruitment | Job Openings, Pipeline, Applicant Details |

## Full Requirements Document

A comprehensive interactive HTML document is available at:
[`docs/requirements.html`](./requirements.html)

It includes use case diagrams, ERD, class diagrams, data models, API endpoint reference, frontend page inventory, route maps, and sequence diagrams — all generated from the actual source code.

