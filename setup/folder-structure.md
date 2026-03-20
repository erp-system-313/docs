# Project Folder Structure

## Root Directory

```
erp-system/
├── frontend/                  # React application
├── backend/                   # Spring Boot application
├── docker-compose.yml         # Container orchestration
├── .env.example               # Environment template
├── .gitignore                 # Git ignore patterns
├── README.md                  # Project documentation
└── docs/                      # Documentation repository
```

---

## Frontend Structure

```
frontend/
├── public/                    # Static public assets
│   ├── index.html
│   ├── manifest.json
│   └── robots.txt
├── src/
│   ├── assets/               # Imported assets
│   │   ├── images/
│   │   └── icons/
│   ├── components/            # Reusable components
│   │   ├── common/            # Generic components
│   │   │   ├── Button/
│   │   │   ├── Card/
│   │   │   ├── DataTable/
│   │   │   ├── Modal/
│   │   │   ├── Loading/
│   │   │   ├── StatusBadge/
│   │   │   └── index.ts
│   │   ├── forms/            # Form components
│   │   │   ├── FormField/
│   │   │   ├── Select/
│   │   │   ├── DatePicker/
│   │   │   └── index.ts
│   │   ├── layout/           # Layout components
│   │   │   ├── Sidebar/
│   │   │   ├── Header/
│   │   │   ├── Footer/
│   │   │   ├── Breadcrumb/
│   │   │   └── index.ts
│   │   └── charts/           # Chart components
│   │       ├── BarChart/
│   │       ├── LineChart/
│   │       ├── PieChart/
│   │       └── index.ts
│   ├── pages/                # Page components (routes)
│   │   ├── common/
│   │   │   ├── Dashboard/
│   │   │   └── Profile/
│   │   ├── inventory/
│   │   │   ├── InventoryDashboard/
│   │   │   ├── ProductList/
│   │   │   ├── ProductDetails/
│   │   │   ├── ProductForm/
│   │   │   └── Categories/
│   │   ├── sales/
│   │   │   ├── SalesDashboard/
│   │   │   ├── SalesOrders/
│   │   │   ├── SalesOrderForm/
│   │   │   ├── CustomersList/
│   │   │   └── CustomerDetails/
│   │   ├── purchasing/
│   │   │   ├── PurchasingDashboard/
│   │   │   ├── PurchaseOrders/
│   │   │   ├── PurchaseOrderForm/
│   │   │   ├── SuppliersList/
│   │   │   └── SupplierDetails/
│   │   ├── finance/
│   │   │   ├── FinanceDashboard/
│   │   │   ├── Invoices/
│   │   │   ├── InvoiceDetails/
│   │   │   ├── InvoiceForm/
│   │   │   ├── ChartOfAccounts/
│   │   │   ├── JournalEntries/
│   │   │   └── FinancialReports/
│   │   ├── hr/
│   │   │   ├── HRDashboard/
│   │   │   ├── EmployeesList/
│   │   │   ├── EmployeeDetails/
│   │   │   ├── Attendance/
│   │   │   └── LeaveRequests/
│   │   ├── admin/
│   │   │   ├── AdminDashboard/
│   │   │   ├── UserManagement/
│   │   │   ├── CompanySettings/
│   │   │   └── AuditLogs/
│   │   ├── auth/
│   │   │   ├── Login/
│   │   │   └── ForgotPassword/
│   │   └── support/
│   │       └── Support/
│   ├── hooks/                # Custom React hooks
│   │   ├── useAuth.ts
│   │   ├── usePermissions.ts
│   │   ├── useToast.ts
│   │   └── index.ts
│   ├── services/              # API services
│   │   ├── apiClient.ts      # Axios instance
│   │   ├── authService.ts
│   │   ├── productService.ts
│   │   ├── customerService.ts
│   │   ├── orderService.ts
│   │   ├── invoiceService.ts
│   │   ├── employeeService.ts
│   │   └── index.ts
│   ├── store/                # State management
│   │   ├── slices/
│   │   │   ├── authSlice.ts
│   │   │   ├── uiSlice.ts
│   │   │   └── index.ts
│   │   ├── store.ts
│   │   └── hooks.ts
│   ├── contexts/             # React contexts
│   │   ├── AuthContext.tsx
│   │   ├── ThemeContext.tsx
│   │   └── ToastContext.tsx
│   ├── types/                # TypeScript types
│   │   ├── api/
│   │   │   ├── requests.ts
│   │   │   ├── responses.ts
│   │   │   └── index.ts
│   │   ├── models/
│   │   │   ├── user.ts
│   │   │   ├── product.ts
│   │   │   ├── order.ts
│   │   │   └── index.ts
│   │   └── index.ts
│   ├── utils/                # Utility functions
│   │   ├── formatters/
│   │   │   ├── formatCurrency.ts
│   │   │   ├── formatDate.ts
│   │   │   └── index.ts
│   │   ├── validators/
│   │   │   ├── emailValidator.ts
│   │   │   └── index.ts
│   │   ├── constants.ts
│   │   └── index.ts
│   ├── styles/               # Global styles
│   │   ├── _variables.scss
│   │   ├── _mixins.scss
│   │   ├── _reset.scss
│   │   ├── _typography.scss
│   │   └── main.scss
│   ├── App.tsx               # Root component
│   ├── AppRoutes.tsx         # Route definitions
│   └── index.tsx             # Entry point
├── tests/                    # Test files
│   ├── unit/
│   ├── integration/
│   └── setup.ts
├── .eslintrc.js
├── .prettierrc
├── tsconfig.json
├── package.json
└── Dockerfile
```

---

## Backend Structure

```
backend/
├── src/
│   ├── main/
│   │   ├── java/
│   │   │   └── com/
│   │   │       └── erp/
│   │   │           ├── ErpApplication.java
│   │   │           ├── common/              # Shared utilities
│   │   │           │   ├── config/
│   │   │           │   │   ├── SecurityConfig.java
│   │   │           │   │   ├── WebConfig.java
│   │   │           │   │   └── CorsConfig.java
│   │   │           │   ├── dto/
│   │   │           │   │   ├── ApiResponse.java
│   │   │           │   │   ├── PageRequest.java
│   │   │           │   │   └── PageResponse.java
│   │   │           │   ├── exception/
│   │   │           │   │   ├── GlobalExceptionHandler.java
│   │   │           │   │   ├── ResourceNotFoundException.java
│   │   │           │   │   └── BusinessException.java
│   │   │           │   └── util/
│   │   │           │       ├── DateUtils.java
│   │   │           │       └── StringUtils.java
│   │   │           ├── inventory/           # Inventory module
│   │   │           │   ├── controller/
│   │   │           │   │   ├── ProductController.java
│   │   │           │   │   └── CategoryController.java
│   │   │           │   ├── service/
│   │   │           │   │   ├── ProductService.java
│   │   │           │   │   └── CategoryService.java
│   │   │           │   ├── repository/
│   │   │           │   │   ├── ProductRepository.java
│   │   │           │   │   └── CategoryRepository.java
│   │   │           │   ├── entity/
│   │   │           │   │   ├── Product.java
│   │   │           │   │   └── Category.java
│   │   │           │   ├── dto/
│   │   │           │   │   ├── ProductDto.java
│   │   │           │   │   ├── CreateProductRequest.java
│   │   │           │   │   └── UpdateProductRequest.java
│   │   │           │   └── mapper/
│   │   │           │       └── ProductMapper.java
│   │   │           ├── sales/               # Sales module
│   │   │           │   ├── controller/
│   │   │           │   │   ├── SalesOrderController.java
│   │   │           │   │   └── CustomerController.java
│   │   │           │   ├── service/
│   │   │           │   ├── repository/
│   │   │           │   ├── entity/
│   │   │           │   ├── dto/
│   │   │           │   └── mapper/
│   │   │           ├── purchasing/          # Purchasing module
│   │   │           ├── finance/             # Finance module
│   │   │           ├── hr/                  # HR module
│   │   │           ├── admin/               # Admin module
│   │   │           │   ├── controller/
│   │   │           │   │   ├── UserController.java
│   │   │           │   │   └── SettingsController.java
│   │   │           │   ├── service/
│   │   │           │   ├── repository/
│   │   │           │   ├── entity/
│   │   │           │   │   ├── User.java
│   │   │           │   │   ├── Role.java
│   │   │           │   │   └── AuditLog.java
│   │   │           │   ├── dto/
│   │   │           │   └── security/
│   │   │           │       ├── JwtTokenProvider.java
│   │   │           │       ├── JwtAuthenticationFilter.java
│   │   │           │       └── UserPrincipal.java
│   │   │           └── auth/                # Authentication module
│   │   │               ├── controller/
│   │   │               │   └── AuthController.java
│   │   │               ├── service/
│   │   │               │   └── AuthService.java
│   │   │               └── dto/
│   │   │                   ├── LoginRequest.java
│   │   │                   ├── LoginResponse.java
│   │   │                   └── RegisterRequest.java
│   │   └── resources/
│   │       ├── application.yml
│   │       ├── application-dev.yml
│   │       ├── application-prod.yml
│   │       ├── db/
│   │       │   └── migration/
│   │       │       ├── V1__initial_schema.sql
│   │       │       ├── V2__add_users.sql
│   │       │       └── V3__add_inventory.sql
│   │       └── logback-spring.xml
│   └── test/
│       └── java/
│           └── com/
│               └── erp/
│                   ├── controller/
│                   ├── service/
│                   └── repository/
├── pom.xml
├── Dockerfile
└── .mvn/
    └── wrapper/
```

---

## Documentation Structure

```
docs/
├── README.md
├── project-overview.md
├── api/
│   ├── overview.md
│   ├── data-models.md
│   ├── endpoints.md
│   ├── authentication.md
│   └── error-handling.md
├── architecture/
│   ├── overview.md
│   ├── pages.md
│   ├── navigation.md
│   └── diagrams/
│       ├── usecase/
│       ├── sequence/
│       ├── class/
│       ├── component/
│       └── activity/
├── assets/
│   └── images/
├── guidelines/
│   ├── git.md
│   ├── frontend.md
│   ├── backend.md
│   ├── linting.md
│   └── testing.md
├── setup/
│   ├── installation.md
│   ├── docker.md
│   ├── environment.md
│   ├── ci-cd.md
│   └── deployment.md
└── team/
    ├── members.md
    └── roles.md
```

---

## Component File Structure

Each component follows this pattern:

```
ComponentName/
├── ComponentName.tsx        # Main component
├── ComponentName.styles.ts  # Styled components or CSS modules
├── ComponentName.types.ts   # Props interface
├── ComponentName.test.tsx   # Unit tests
└── index.ts                 # Barrel export
```

Example:

```
ProductList/
├── ProductList.tsx
├── ProductList.styles.ts
├── ProductList.types.ts
├── ProductList.test.tsx
├── ProductListItem.tsx
└── index.ts
```

---

## Module Structure Pattern

Each backend module follows:

```
module/
├── controller/    # REST endpoints
├── service/      # Business logic
├── repository/   # Data access
├── entity/       # JPA entities
├── dto/          # Request/Response DTOs
├── mapper/       # Entity-DTO mapping
└── exception/    # Module-specific exceptions
```

---

## Key Principles

1. **Flat over nested** - Keep directory depth reasonable
2. **Barrel exports** - Use `index.ts` for clean imports
3. **Colocation** - Keep related files together (test next to source)
4. **Shared vs Local** - Extract truly reusable code to `common/`
5. **Convention over configuration** - Follow the patterns above
