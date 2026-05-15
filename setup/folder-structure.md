# Project Folder Structure

## Root Directory

```
erp-system/
├── web/                       # React application (Vite)
├── service/                   # Spring Boot application
├── docs/                      # Documentation repository
└── .github/                   # GitHub Actions CI/CD
```

---

## Frontend Structure

```
web/
├── public/                    # Static public assets
│   ├── index.html
│   ├── manifest.json
│   └── robots.txt
├── src/
│   ├── api/                  # Type-safe API client (separate from services/)
│   │   ├── client.ts         # Axios instance (baseURL: /api)
│   │   ├── endpoints.ts      # Endpoint path constants
│   │   └── index.ts
│   ├── assets/               # Imported assets
│   │   ├── hero.png
│   │   ├── react.svg
│   │   └── vite.svg
│   ├── components/            # Reusable components
│   │   ├── common/            # Generic components
│   │   │   ├── Autocomplete/
│   │   │   ├── DataTable/
│   │   │   ├── FormField/
│   │   │   ├── LineItemTable/
│   │   │   ├── StatusBadge/
│   │   │   ├── TabPanel/
│   │   │   └── index.ts
│   │   ├── Layout/           # Layout components
│   │   │   ├── MainLayout/
│   │   │   ├── Sidebar/
│   │   │   └── index.ts
│   │   └── index.ts
│   ├── contexts/             # React contexts
│   │   └── AuthContext.tsx
│   ├── data/                 # Static/mock data
│   │   └── mockEmployees.ts
│   ├── hooks/                # Custom React hooks
│   │   ├── useAccounts.ts
│   │   ├── useAttendance.ts
│   │   ├── useAuditLogs.ts
│   │   ├── useCategories.ts
│   │   ├── useCustomer.ts
│   │   ├── useCustomers.ts
│   │   ├── useDashboardStats.ts
│   │   ├── useEmployee.ts
│   │   ├── useEmployees.ts
│   │   ├── useInvoice.ts
│   │   ├── useInvoices.ts
│   │   ├── useJournalEntries.ts
│   │   ├── useJournalEntry.ts
│   │   ├── useLeaveRequests.ts
│   │   ├── useProducts.ts
│   │   ├── usePurchaseOrders.ts
│   │   ├── useSalesOrder.ts
│   │   ├── useSalesOrders.ts
│   │   ├── useSettings.ts
│   │   ├── useSuppliers.ts
│   │   ├── useUsers.ts
│   │   └── index.ts
│   ├── mocks/                # Mock data for development
│   │   ├── accountsMockData.ts
│   │   ├── invoicesMockData.ts
│   │   ├── journalMockData.ts
│   │   ├── productsMockData.ts
│   │   └── salesMockData.ts
│   ├── pages/                # Page components (route-level, organized by module)
│   │   ├── admin/
│   │   │   ├── AuditLogs/
│   │   │   ├── Settings/
│   │   │   └── Users/
│   │   ├── auth/
│   │   │   └── Login/
│   │   ├── common/
│   │   │   ├── Dashboard/
│   │   │   └── Profile/
│   │   ├── finance/
│   │   │   ├── ChartOfAccounts/
│   │   │   ├── InvoiceDetails/
│   │   │   ├── InvoiceForm/
│   │   │   ├── InvoicesList/
│   │   │   ├── JournalEntries/
│   │   │   └── JournalEntryForm/
│   │   ├── hr/
│   │   │   ├── Attendance/
│   │   │   ├── EmployeeDetails/
│   │   │   ├── EmployeesList/
│   │   │   └── LeaveRequests/
│   │   ├── inventory/
│   │   │   ├── CategoryListPage.tsx       # Flat files (no subdirectory)
│   │   │   ├── CreateProductPage.tsx
│   │   │   ├── EditProductPage.tsx
│   │   │   ├── ProductDetailsPage.tsx
│   │   │   └── ProductListPage.tsx
│   │   ├── purchasing/
│   │   │   ├── CreatePurchaseOrderPage.tsx
│   │   │   ├── PurchaseOrderListPage.tsx
│   │   │   ├── SupplierDetailsPage.tsx
│   │   │   └── SupplierListPage.tsx
│   │   └── sales/
│   │       ├── CustomerDetails/
│   │       ├── CustomersList/
│   │       ├── SalesOrderDetails/
│   │       ├── SalesOrderForm/
│   │       └── SalesOrdersList/
│   ├── services/              # API service modules (baseURL: /api/v1)
│   │   ├── apiClient.ts      # Axios instance for /api/v1
│   │   ├── auditLogsService.ts
│   │   ├── authService.ts
│   │   ├── dashboardService.ts
│   │   ├── financeService.ts
│   │   ├── hrService.ts
│   │   ├── inventoryService.ts
│   │   ├── purchasingService.ts
│   │   ├── salesService.ts
│   │   ├── settingsService.ts
│   │   └── usersService.ts
│   ├── types/                # TypeScript types
│   │   ├── category.types.ts
│   │   ├── finance.ts
│   │   ├── hr.ts
│   │   ├── index.ts
│   │   ├── models/
│   │   │   └── employee.ts
│   │   ├── product.types.ts
│   │   ├── purchaseOrder.types.ts
│   │   ├── sales.ts
│   │   └── supplier.types.ts
│   ├── App.tsx               # Root component
│   ├── AppRoutes.tsx         # Route definitions
│   ├── App.css
│   ├── index.css
│   └── main.tsx              # Entry point
├── .eslintrc.js
├── .prettierrc
├── index.html
├── tsconfig.json
├── vite.config.ts
├── package.json
└── Dockerfile
```

---

## Backend Structure

```
service/
├── docker-compose.yml           # Container orchestration (PostgreSQL + Redis)
├── src/
│   ├── main/
│   │   ├── java/
│   │   │   └── com/
│   │   │       └── erp/
│   │   │           ├── ErpApplication.java
│   │   │           ├── config/
│   │   │           │   ├── SecurityConfig.java
│   │   │           │   └── WebConfig.java
│   │   │           ├── common/
│   │   │           │   ├── annotation/
│   │   │           │   │   └── Auditable.java
│   │   │           │   ├── dto/
│   │   │           │   │   ├── ApiResponse.java
│   │   │           │   │   └── PageResponse.java
│   │   │           │   ├── exception/
│   │   │           │   │   ├── GlobalExceptionHandler.java
│   │   │           │   │   ├── ResourceNotFoundException.java
│   │   │           │   │   └── BusinessException.java
│   │   │           │   └── util/
│   │   │           │       ├── DateUtils.java
│   │   │           │       └── StringUtils.java
│   │   │           ├── auth/
│   │   │           │   ├── controller/
│   │   │           │   │   └── AuthController.java
│   │   │           │   ├── dto/
│   │   │           │   │   ├── LoginRequest.java
│   │   │           │   │   ├── LoginResponse.java
│   │   │           │   │   ├── RegisterRequest.java
│   │   │           │   │   ├── RefreshTokenRequest.java
│   │   │           │   │   ├── ResetPasswordRequest.java
│   │   │           │   │   ├── ForgotPasswordRequest.java
│   │   │           │   │   └── TokenResponse.java
│   │   │           │   ├── security/
│   │   │           │   │   ├── JwtTokenProvider.java
│   │   │           │   │   ├── JwtAuthenticationFilter.java
│   │   │           │   │   ├── CurrentUserUtil.java
│   │   │           │   │   ├── UserDetailsServiceImpl.java
│   │   │           │   │   └── UserPrincipal.java
│   │   │           │   └── service/
│   │   │           │       └── AuthService.java
│   │   │           ├── admin/
│   │   │           │   ├── aspect/
│   │   │           │   │   └── AuditAspect.java
│   │   │           │   ├── controller/
│   │   │           │   │   ├── UserController.java
│   │   │           │   │   ├── SettingsController.java
│   │   │           │   │   ├── AuditLogController.java
│   │   │           │   │   └── DashboardController.java
│   │   │           │   ├── dto/
│   │   │           │   │   ├── UserDto.java
│   │   │           │   │   ├── CreateUserRequest.java
│   │   │           │   │   ├── UpdateUserRequest.java
│   │   │           │   │   ├── AuditLogDto.java
│   │   │           │   │   ├── SettingsDto.java
│   │   │           │   │   ├── DashboardStatsDto.java
│   │   │           │   │   └── UpdateSettingsRequest.java
│   │   │           │   ├── entity/
│   │   │           │   │   ├── User.java
│   │   │           │   │   ├── Role.java
│   │   │           │   │   ├── AuditLog.java
│   │   │           │   │   └── Settings.java
│   │   │           │   ├── repository/
│   │   │           │   │   ├── UserRepository.java
│   │   │           │   │   ├── RoleRepository.java
│   │   │           │   │   ├── AuditLogRepository.java
│   │   │           │   │   └── SettingsRepository.java
│   │   │           │   └── service/
│   │   │           │       ├── UserService.java
│   │   │           │       ├── SettingsService.java
│   │   │           │       ├── AuditLogService.java
│   │   │           │       └── DashboardService.java
│   │   │           ├── inventory/
│   │   │           │   ├── controller/
│   │   │           │   │   ├── ProductController.java
│   │   │           │   │   └── CategoryController.java
│   │   │           │   ├── dto/
│   │   │           │   │   ├── ProductDto.java
│   │   │           │   │   ├── CategoryDto.java
│   │   │           │   │   ├── CreateProductRequest.java
│   │   │           │   │   ├── UpdateProductRequest.java
│   │   │           │   │   ├── CreateCategoryRequest.java
│   │   │           │   │   └── UpdateCategoryRequest.java
│   │   │           │   ├── entity/
│   │   │           │   │   ├── Product.java
│   │   │           │   │   └── Category.java
│   │   │           │   ├── repository/
│   │   │           │   │   ├── ProductRepository.java
│   │   │           │   │   └── CategoryRepository.java
│   │   │           │   └── service/
│   │   │           │       ├── ProductService.java
│   │   │           │       └── CategoryService.java
│   │   │           ├── sales/
│   │   │           │   ├── controller/
│   │   │           │   │   ├── SalesOrderController.java
│   │   │           │   │   └── CustomerController.java
│   │   │           │   ├── dto/
│   │   │           │   │   ├── CreateSalesOrderRequest.java
│   │   │           │   │   ├── CreateCustomerRequest.java
│   │   │           │   │   ├── UpdateCustomerRequest.java
│   │   │           │   │   ├── CustomerDto.java
│   │   │           │   │   └── SalesOrderDto.java
│   │   │           │   ├── entity/
│   │   │           │   │   ├── SalesOrder.java
│   │   │           │   │   ├── SalesOrderLine.java
│   │   │           │   │   └── Customer.java
│   │   │           │   ├── repository/
│   │   │           │   │   ├── SalesOrderRepository.java
│   │   │           │   │   └── CustomerRepository.java
│   │   │           │   └── service/
│   │   │           │       ├── SalesOrderService.java
│   │   │           │       ├── CustomerService.java
│   │   │           │       └── ProductClientStub.java
│   │   │           ├── purchasing/
│   │   │           │   ├── controller/
│   │   │           │   │   ├── PurchaseOrderController.java
│   │   │           │   │   ├── SupplierController.java
│   │   │           │   │   └── StockMovementController.java
│   │   │           │   ├── dto/
│   │   │           │   │   ├── CreateSupplierRequest.java
│   │   │           │   │   ├── SupplierDto.java
│   │   │           │   │   └── PurchaseOrderDto.java
│   │   │           │   ├── entity/
│   │   │           │   │   ├── PurchaseOrder.java
│   │   │           │   │   ├── PurchaseOrderLine.java
│   │   │           │   │   ├── Supplier.java
│   │   │           │   │   └── StockMovement.java
│   │   │           │   ├── repository/
│   │   │           │   │   ├── PurchaseOrderRepository.java
│   │   │           │   │   ├── SupplierRepository.java
│   │   │           │   │   └── StockMovementRepository.java
│   │   │           │   └── service/
│   │   │           │       ├── PurchaseOrderService.java
│   │   │           │       └── SupplierService.java
│   │   │           ├── finance/
│   │   │           │   ├── controller/
│   │   │           │   │   ├── InvoiceController.java
│   │   │           │   │   ├── AccountController.java
│   │   │           │   │   └── JournalEntryController.java
│   │   │           │   ├── dto/
│   │   │           │   │   ├── InvoiceDto.java
│   │   │           │   │   ├── InvoiceLineDto.java
│   │   │           │   │   ├── PaymentDto.java
│   │   │           │   │   ├── AccountDto.java
│   │   │           │   │   ├── CreateAccountRequest.java
│   │   │           │   │   ├── UpdateAccountRequest.java
│   │   │           │   │   ├── CreateInvoiceRequest.java
│   │   │           │   │   ├── CreatePaymentRequest.java
│   │   │           │   │   ├── JournalEntryDto.java
│   │   │           │   │   ├── JournalEntryLineDto.java
│   │   │           │   │   └── CreateJournalEntryRequest.java
│   │   │           │   ├── entity/
│   │   │           │   │   ├── Invoice.java
│   │   │           │   │   ├── InvoiceLine.java
│   │   │           │   │   ├── Payment.java
│   │   │           │   │   ├── Account.java
│   │   │           │   │   ├── JournalEntry.java
│   │   │           │   │   └── JournalEntryLine.java
│   │   │           │   ├── repository/
│   │   │           │   │   ├── InvoiceRepository.java
│   │   │           │   │   ├── PaymentRepository.java
│   │   │           │   │   ├── AccountRepository.java
│   │   │           │   │   ├── JournalEntryRepository.java
│   │   │           │   │   └── JournalEntryLineRepository.java
│   │   │           │   └── service/
│   │   │           │       ├── InvoiceService.java
│   │   │           │       ├── AccountService.java
│   │   │           │       └── JournalEntryService.java
│   │   │           └── hr/
│   │   │               ├── controller/
│   │   │               │   ├── EmployeeController.java
│   │   │               │   ├── AttendanceController.java
│   │   │               │   ├── LeaveController.java
│   │   │               │   └── LeaveBalanceController.java
│   │   │               ├── dto/
│   │   │               │   ├── CreateEmployeeRequest.java
│   │   │               │   ├── EmployeeDto.java
│   │   │               │   ├── AttendanceDto.java
│   │   │               │   ├── LeaveRequestDto.java
│   │   │               │   └── LeaveBalanceDto.java
│   │   │               ├── entity/
│   │   │               │   ├── Employee.java
│   │   │               │   ├── Attendance.java
│   │   │               │   ├── LeaveRequest.java
│   │   │               │   └── LeaveBalance.java
│   │   │               ├── repository/
│   │   │               │   ├── EmployeeRepository.java
│   │   │               │   ├── AttendanceRepository.java
│   │   │               │   ├── LeaveRequestRepository.java
│   │   │               │   └── LeaveBalanceRepository.java
│   │   │               └── service/
│   │   │                   ├── EmployeeService.java
│   │   │                   └── AttendanceService.java
│   │   └── resources/
│   │       ├── application.yml
│   │       ├── application-dev.yml
│   │       ├── application-prod.yml
│   │       ├── application-test.yml       (in src/test/resources/)
│   │       ├── db/
│   │       │   └── migration/
│   │       │       ├── V1__admin_schema.sql
│   │       │       ├── V2__hr_schema.sql
│   │       │       ├── V3__inventory.sql
│   │       │       ├── V4__purchasing.sql
│   │       │       ├── V5__sales.sql
│   │       │       ├── V6__finance.sql
│   │       │       ├── V7__seed_data.sql
│   │       │       ├── V8__test_users.sql
│   │       │       ├── V9__fix_attendance_schema.sql
│   │       │       └── ...
│   │       └── logback-spring.xml
│   ├── test/
│   │   ├── java/
│   │   │   └── com/
│   │   │       └── erp/
│   │   │           ├── admin/
│   │   │           ├── auth/
│   │   │           ├── finance/
│   │   │           ├── hr/
│   │   │           ├── inventory/
│   │   │           ├── purchasing/
│   │   │           └── sales/
│   │   └── resources/
│   │       └── application-test.yml
├── pom.xml
├── flake.nix
├── .env.example
└── .mvn/
```

---

## Documentation Structure

```
docs/
├── README.md
├── project-overview.md
├── api/
│   ├── data-models.md
│   └── endpoints.md
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
│   └── linting.md
└── setup/
    └── folder-structure.md
```

---

## Component File Structure

Most components follow this pattern:

```
ComponentName/
├── ComponentName.tsx        # Main component
├── ComponentName.module.css # CSS Module styles (or plain .css)
└── index.ts                 # Barrel export
```

Some modules use flat file naming instead of subdirectories:

```
pages/inventory/
├── ProductListPage.tsx      # No subdirectory
├── ProductDetailsPage.tsx
├── CreateProductPage.tsx
├── EditProductPage.tsx
└── CategoryListPage.tsx
```

---

## Module Structure Pattern (Backend)

Each backend module follows:

```
module/
├── controller/    # REST endpoints
├── service/      # Business logic
├── repository/   # Data access
├── entity/       # JPA entities
└── dto/          # Request/Response DTOs
```

**Note**: There is no separate `mapper/` package. Entity-to-DTO conversion is done via static methods on DTOs or private methods in services. There is no `exception/` per module — exceptions are shared in `common/exception/`.

---

## Key Principles

1. **Flat over nested** - Keep directory depth reasonable
2. **Barrel exports** - Use `index.ts` for clean imports
3. **Colocation** - Keep related files together (CSS module next to component)
4. **Shared vs Local** - Extract truly reusable code to `common/`
5. **Convention over configuration** - Follow the patterns above
