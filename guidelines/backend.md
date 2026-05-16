# Backend Guidelines

## Project Structure

```
service/
├── src/
│   └── main/
│       └── java/
│           └── com/
│               └── erp/
│                   ├── config/          # Configuration classes
│                   │   ├── SecurityConfig.java
│                   │   └── WebConfig.java
│                   ├── common/           # Shared utilities
│                   │   ├── annotation/   # Custom annotations (Auditable)
│                   │   ├── dto/          # ApiResponse, PageResponse
│                   │   ├── exception/    # GlobalExceptionHandler, ResourceNotFoundException, BusinessException
│                   │   └── util/         # DateUtils, StringUtils
│                   ├── auth/            # Authentication module
│                   │   ├── controller/   # AuthController
│                   │   ├── dto/          # LoginRequest, LoginResponse, RegisterRequest, etc.
│                   │   ├── security/     # JwtTokenProvider, JwtAuthenticationFilter, CurrentUserUtil, UserPrincipal
│                   │   └── service/      # AuthService
│                   ├── admin/           # Admin module
│                   │   ├── aspect/      # AuditAspect
│                   │   ├── controller/  # UserController, SettingsController, AuditLogController, DashboardController
│                   │   ├── dto/         # UserDto, CreateUserRequest, UpdateUserRequest, etc.
│                   │   ├── entity/      # User, Role, AuditLog, Settings
│                   │   ├── repository/  # UserRepository, RoleRepository, etc.
│                   │   └── service/     # UserService, SettingsService, AuditLogService, DashboardService
│                   ├── inventory/       # Inventory module
│                   │   ├── controller/  # ProductController, CategoryController
│                   │   ├── dto/         # ProductDto, CreateProductRequest, etc.
│                   │   ├── entity/      # Product, Category
│                   │   ├── repository/  # ProductRepository, CategoryRepository
│                   │   └── service/     # ProductService, CategoryService
│                   ├── sales/           # Sales module
│                   │   ├── controller/  # SalesOrderController, CustomerController
│                   │   ├── dto/         # CreateSalesOrderRequest, CreateCustomerRequest, etc.
│                   │   ├── entity/      # SalesOrder, SalesOrderLine, Customer
│                   │   ├── repository/  # SalesOrderRepository, CustomerRepository
│                   │   └── service/     # SalesOrderService, CustomerService, ProductClientStub
│                   ├── purchasing/      # Purchasing module
│                   │   ├── controller/  # PurchaseOrderController, SupplierController, StockMovementController
│                   │   ├── dto/         # CreateSupplierRequest, SupplierDto, etc.
│                   │   ├── entity/      # PurchaseOrder, PurchaseOrderLine, Supplier, StockMovement
│                   │   ├── repository/  # PurchaseOrderRepository, SupplierRepository
│                   │   └── service/     # PurchaseOrderService, SupplierService
│                   ├── finance/         # Finance module
│                   │   ├── controller/  # InvoiceController, AccountController, JournalEntryController
│                   │   ├── dto/         # InvoiceDto, InvoiceLineDto, PaymentDto, AccountDto, JournalEntryDto, etc.
│                   │   ├── entity/      # Invoice, InvoiceLine, Payment, Account, JournalEntry, JournalEntryLine
│                   │   ├── repository/  # InvoiceRepository, AccountRepository, JournalEntryRepository, PaymentRepository
│                   │   └── service/     # InvoiceService, AccountService, JournalEntryService
│                   └── hr/             # HR module
│                       ├── controller/ # EmployeeController, AttendanceController, LeaveController, LeaveBalanceController
│                       ├── dto/        # CreateEmployeeRequest, etc.
│                       ├── entity/     # Employee, Attendance, LeaveRequest, LeaveBalance
│                       ├── repository/ # EmployeeRepository, AttendanceRepository, LeaveRequestRepository, LeaveBalanceRepository
│                       └── service/    # EmployeeService, AttendanceService
├── src/test/                    # Unit tests
├── resources/
│   ├── application.yml          # Application config
│   ├── application-dev.yml
│   ├── application-prod.yml
│   └── db/migration/            # Flyway migrations
├── pom.xml                      # Maven dependencies
├── flake.nix                    # Nix flake
├── .env.example
└── Dockerfile
```

## Package Organization

Each module gets its own package:

```
com.erp.inventory.*
com.erp.sales.*
com.erp.hr.*
com.erp.admin.*
com.erp.finance.*
com.erp.purchasing.*
com.erp.auth.*
com.erp.common.*
com.erp.config.*
```

## Naming Conventions

| Type | Convention | Example |
|------|------------|---------|
| Classes | PascalCase | `UserController` |
| Methods | camelCase | `getUserById` |
| Variables | camelCase | `userId`, `isActive` |
| Constants | UPPER_SNAKE_CASE | `MAX_RETRY_COUNT` |
| Tables | snake_case (plural) | `sales_orders` |
| Columns | snake_case | `created_at` |
| Packages | lowercase | `com.erp.controller` |
| REST Endpoints | kebab-case, plural nouns | `/api/v1/users`, `/api/v1/sales-orders` |

## Layer Structure

### 1. Controller Layer

```java
@RestController
@RequestMapping("/api/v1/users")
@RequiredArgsConstructor
@Slf4j
public class UserController {

    private final UserService userService;
    private final CurrentUserUtil currentUserUtil;

    @GetMapping
    public ResponseEntity<ApiResponse<PageResponse<UserDto>>> getAll(
            @RequestParam(defaultValue = "0") int page,
            @RequestParam(defaultValue = "20") int size,
            @RequestParam(required = false) String search,
            @RequestParam(required = false) String roleName,
            @RequestParam(required = false) Boolean isActive) {

        log.info("Fetching users - page: {}, size: {}", page, size);
        PageResponse<UserDto> users = userService.findAll(page, size, search, roleName, isActive);
        return ResponseEntity.ok(ApiResponse.success(users));
    }

    @GetMapping("/{id}")
    public ResponseEntity<ApiResponse<UserDto>> getById(@PathVariable Long id) {
        UserDto user = userService.findById(id);
        return ResponseEntity.ok(ApiResponse.success(user));
    }

    @GetMapping("/email/{email}")
    public ResponseEntity<ApiResponse<UserDto>> getByEmail(@PathVariable String email) {
        UserDto user = userService.findByEmail(email);
        return ResponseEntity.ok(ApiResponse.success(user));
    }

    @PostMapping
    public ResponseEntity<ApiResponse<UserDto>> create(
            @Valid @RequestBody CreateUserRequest request,
            HttpServletRequest httpRequest) {
        Long currentUserId = currentUserUtil.getCurrentUserId();
        String ipAddress = httpRequest.getRemoteAddr();
        UserDto created = userService.create(request, currentUserId, ipAddress);
        return ResponseEntity.status(HttpStatus.CREATED)
                .body(ApiResponse.success(created, "User created successfully"));
    }

    @PutMapping("/{id}")
    public ResponseEntity<ApiResponse<UserDto>> update(
            @PathVariable Long id,
            @Valid @RequestBody UpdateUserRequest request,
            HttpServletRequest httpRequest) {
        Long currentUserId = currentUserUtil.getCurrentUserId();
        String ipAddress = httpRequest.getRemoteAddr();
        UserDto updated = userService.update(id, request, currentUserId, ipAddress);
        return ResponseEntity.ok(ApiResponse.success(updated, "User updated successfully"));
    }

    @DeleteMapping("/{id}")
    public ResponseEntity<ApiResponse<Void>> delete(
            @PathVariable Long id,
            HttpServletRequest httpRequest) {
        Long currentUserId = currentUserUtil.getCurrentUserId();
        String ipAddress = httpRequest.getRemoteAddr();
        userService.delete(id, currentUserId, ipAddress);
        return ResponseEntity.status(HttpStatus.NO_CONTENT).build();
    }
}
```

### 2. Service Layer

```java
@Service
@Transactional
@RequiredArgsConstructor
@Slf4j
public class UserService {

    private final UserRepository userRepository;
    private final RoleRepository roleRepository;
    private final PasswordEncoder passwordEncoder;
    private final AuditLogService auditLogService;
    private final CurrentUserUtil currentUserUtil;

    public PageResponse<UserDto> findAll(int page, int size, String search, String roleName, Boolean isActive) {
        Pageable pageable = PageRequest.of(page, size, Sort.by("createdAt").descending());

        Page<User> users;
        if (search != null && !search.isEmpty()) {
            users = userRepository.search(search, pageable);
        } else {
            users = userRepository.findAll(pageable);
        }

        return PageResponse.from(users.map(this::toDto));
    }

    public UserDto findById(Long id) {
        User user = userRepository.findByIdWithRole(id)
                .orElseThrow(() -> new ResourceNotFoundException("User", id));
        return toDto(user);
    }

    @Transactional
    public UserDto create(CreateUserRequest request, Long currentUserId, String ipAddress) {
        if (userRepository.existsByEmail(request.getEmail())) {
            throw new BusinessException("USER_001", "Email already exists");
        }

        User user = User.builder()
                .email(request.getEmail())
                .passwordHash(passwordEncoder.encode(request.getPassword()))
                .firstName(request.getFirstName())
                .lastName(request.getLastName())
                .isActive(true)
                .build();

        if (request.getRoleId() != null) {
            Role role = roleRepository.findById(request.getRoleId())
                    .orElseThrow(() -> new ResourceNotFoundException("Role", request.getRoleId()));
            user.setRole(role);
        }

        user = userRepository.save(user);
        log.info("Created user with id: {}", user.getId());

        auditLogService.log(currentUserId, "CREATE", "User", user.getId(), null, ipAddress, "User created");

        return toDto(user);
    }

    // DTO conversion is done via private method, not a separate mapper class
    private UserDto toDto(User user) {
        return UserDto.builder()
                .id(user.getId())
                .email(user.getEmail())
                .firstName(user.getFirstName())
                .lastName(user.getLastName())
                .fullName(user.getFullName())
                .roleId(user.getRole() != null ? user.getRole().getId() : null)
                .roleName(user.getRole() != null ? user.getRole().getName() : null)
                .employeeId(user.getEmployee() != null ? user.getEmployee().getId() : null)
                .isActive(user.getIsActive())
                .lastLoginAt(user.getLastLoginAt())
                .createdAt(user.getCreatedAt())
                .build();
    }
}
```

### 3. Repository Layer

```java
@Repository
public interface UserRepository extends JpaRepository<User, Long> {

    Optional<User> findByEmail(String email);

    boolean existsByEmail(String email);

    @Query("SELECT u FROM User u LEFT JOIN FETCH u.role WHERE u.id = :id")
    Optional<User> findByIdWithRole(@Param("id") Long id);

    @Query("SELECT u FROM User u LEFT JOIN FETCH u.role WHERE u.email = :email")
    Optional<User> findByEmailWithRole(@Param("email") String email);

    @Query("SELECT u FROM User u WHERE u.isActive = true")
    List<User> findAllActive();

    @Query("SELECT u FROM User u WHERE u.isActive = true")
    Page<User> findAllActive(Pageable pageable);

    @Query("SELECT u FROM User u WHERE u.role.name = :roleName")
    Page<User> findByRoleName(@Param("roleName") String roleName, Pageable pageable);

    @Query("SELECT u FROM User u WHERE LOWER(u.firstName) LIKE LOWER(CONCAT('%', :search, '%')) " +
           "OR LOWER(u.lastName) LIKE LOWER(CONCAT('%', :search, '%')) " +
           "OR LOWER(u.email) LIKE LOWER(CONCAT('%', :search, '%'))")
    Page<User> search(@Param("search") String search, Pageable pageable);
}
```

### 4. Entity Layer

```java
@Entity
@Table(name = "users")
@Getter
@Setter
@NoArgsConstructor
@AllArgsConstructor
@Builder
public class User {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @Column(nullable = false, unique = true, length = 255)
    private String email;

    @Column(name = "password_hash", nullable = false, length = 255)
    private String passwordHash;

    @Column(name = "first_name", length = 100)
    private String firstName;

    @Column(name = "last_name", length = 100)
    private String lastName;

    @ManyToOne(fetch = FetchType.LAZY)
    @JoinColumn(name = "role_id")
    private Role role;                          // Entity reference, not enum

    @OneToOne(fetch = FetchType.LAZY)
    @JoinColumn(name = "employee_id")
    private Employee employee;

    @Column(name = "is_active", nullable = false)
    @Builder.Default
    private Boolean isActive = true;

    @Column(name = "last_login_at")
    private LocalDateTime lastLoginAt;

    @Column(name = "reset_token")
    private String resetToken;

    @Column(name = "reset_token_expires_at")
    private LocalDateTime resetTokenExpiresAt;

    @CreationTimestamp
    @Column(name = "created_at", nullable = false, updatable = false)
    private LocalDateTime createdAt;

    @UpdateTimestamp
    @Column(name = "updated_at")
    private LocalDateTime updatedAt;

    public String getFullName() {
        return firstName + " " + lastName;
    }
}
```

## DTO Pattern

**Note**: There are no separate `*Mapper` classes. Entity-to-DTO conversion is done via static `fromEntity()` methods on DTOs or private `toDto()` methods in service classes.

### Request DTO

```java
@Getter
@Setter
@NoArgsConstructor
@AllArgsConstructor
@Builder
public class CreateUserRequest {

    @NotBlank(message = "First name is required")
    @Size(min = 2, max = 100)
    private String firstName;

    @NotBlank(message = "Last name is required")
    @Size(min = 2, max = 100)
    private String lastName;

    @NotBlank(message = "Email is required")
    @Email(message = "Invalid email format")
    private String email;

    @NotBlank(message = "Password is required")
    @Size(min = 8, message = "Password must be at least 8 characters")
    private String password;

    private Long roleId;
}
```

### Response DTO

```java
@Data
@NoArgsConstructor
@AllArgsConstructor
@Builder
public class UserDto {
    private Long id;
    private String email;
    private String firstName;
    private String lastName;
    private String fullName;
    private String roleName;
    private Long roleId;
    private Long employeeId;
    private boolean isActive;
    private LocalDateTime lastLoginAt;
    private LocalDateTime createdAt;
}
```

## API Response Format

### Standard Response Wrapper

```java
@Data
@NoArgsConstructor
@AllArgsConstructor
@Builder
public class ApiResponse<T> {

    private boolean success;
    private String message;
    private T data;
    private ErrorInfo error;

    public static <T> ApiResponse<T> success(T data) {
        return ApiResponse.<T>builder()
                .success(true)
                .data(data)
                .build();
    }

    public static <T> ApiResponse<T> success(T data, String message) {
        return ApiResponse.<T>builder()
                .success(true)
                .message(message)
                .data(data)
                .build();
    }

    public static <T> ApiResponse<T> error(String code, String message) {
        return ApiResponse.<T>builder()
                .success(false)
                .error(ErrorInfo.builder().code(code).message(message).build())
                .build();
    }
}
```

### Paginated Response

```java
@Data
@NoArgsConstructor
@AllArgsConstructor
@Builder
public class PageResponse<T> {

    private List<T> content;
    private long totalElements;
    private int totalPages;
    private int size;
    private int number;
    private boolean first;
    private boolean last;

    public static <T> PageResponse<T> from(Page<T> page) {
        return PageResponse.<T>builder()
                .content(page.getContent())
                .totalElements(page.getTotalElements())
                .totalPages(page.getTotalPages())
                .size(page.getSize())
                .number(page.getNumber())
                .first(page.isFirst())
                .last(page.isLast())
                .build();
    }
}
```

## Exception Handling

### Custom Exceptions

```java
@ResponseStatus(HttpStatus.NOT_FOUND)
public class ResourceNotFoundException extends RuntimeException {

    private final String resource;
    private final Object identifier;

    public ResourceNotFoundException(String resource, Object identifier) {
        super(String.format("%s not found with identifier: %s", resource, identifier));
        this.resource = resource;
        this.identifier = identifier;
    }
}

@ResponseStatus(HttpStatus.BAD_REQUEST)
public class BusinessException extends RuntimeException {

    private final String code;

    public BusinessException(String code, String message) {
        super(message);
        this.code = code;
    }
}
```

### Global Exception Handler

```java
@RestControllerAdvice
@RequiredArgsConstructor
@Slf4j
public class GlobalExceptionHandler {

    @ExceptionHandler(ResourceNotFoundException.class)
    public ResponseEntity<ApiResponse<Void>> handleNotFound(ResourceNotFoundException ex) {
        log.warn("Resource not found: {}", ex.getMessage());
        return ResponseEntity.status(HttpStatus.NOT_FOUND)
                .body(ApiResponse.error("NOT_FOUND", ex.getMessage()));
    }

    @ExceptionHandler(BusinessException.class)
    public ResponseEntity<ApiResponse<Void>> handleBusiness(BusinessException ex) {
        log.warn("Business error: {}", ex.getMessage());
        return ResponseEntity.status(HttpStatus.BAD_REQUEST)
                .body(ApiResponse.error(ex.getCode(), ex.getMessage()));
    }

    @ExceptionHandler(MethodArgumentNotValidException.class)
    public ResponseEntity<ApiResponse<Void>> handleValidation(
            MethodArgumentNotValidException ex) {

        List<FieldError> errors = ex.getBindingResult().getFieldErrors().stream()
                .map(e -> new FieldError(e.getField(), e.getDefaultMessage()))
                .collect(Collectors.toList());

        ErrorInfo errorInfo = ErrorInfo.builder()
                .code("VALIDATION_ERROR")
                .message("Validation failed")
                .fieldErrors(errors)
                .build();

        return ResponseEntity.status(HttpStatus.BAD_REQUEST)
                .body(ApiResponse.<Void>builder()
                        .success(false)
                        .error(errorInfo)
                        .build());
    }

    @ExceptionHandler(Exception.class)
    public ResponseEntity<ApiResponse<Void>> handleGeneral(Exception ex) {
        log.error("Unexpected error", ex);
        return ResponseEntity.status(HttpStatus.INTERNAL_SERVER_ERROR)
                .body(ApiResponse.error("INTERNAL_ERROR", "An unexpected error occurred"));
    }
}
```

## Validation Rules

### Common Annotations

| Annotation | Usage |
|------------|-------|
| `@NotNull` | Value cannot be null |
| `@NotBlank` | String cannot be blank |
| `@NotEmpty` | Collection/string cannot be empty |
| `@Size` | Length/size constraints |
| `@Email` | Valid email format |
| `@Min/@Max` | Numeric constraints |
| `@Past/@Future` | Date constraints |
| `@Pattern` | Regex pattern |
| `@Valid` | Nested object validation |

## Logging Standards

```java
@Slf4j
public class UserService {

    public void performAction() {
        // Use appropriate log levels
        log.debug("Debug info for troubleshooting");
        log.info("User action: {}", userId);        // User actions
        log.warn("Potential issue: {}", detail);    // Recoverable issues
        log.error("Error occurred: {}", ex.getMessage(), ex);  // Errors

        // Don't log sensitive data
        // log.info("Password: {}", password);  // BAD
    }
}
```

## Security Guidelines

### JWT Authentication (jjwt 0.12+)

```java
@Component
@Slf4j
public class JwtTokenProvider {

    @Value("${jwt.secret}")
    private String jwtSecret;

    @Value("${jwt.expiration}")
    private long jwtExpiration;

    private SecretKey getSigningKey() {
        byte[] keyBytes = Decoders.BASE64.decode(
            java.util.Base64.getEncoder().encodeToString(jwtSecret.getBytes()));
        return Keys.hmacShaKeyFor(keyBytes);
    }

    public String generateAccessToken(Authentication authentication) {
        UserPrincipal userPrincipal = (UserPrincipal) authentication.getPrincipal();
        Date now = new Date();
        Date expiryDate = new Date(now.getTime() + jwtExpiration);

        return Jwts.builder()
                .subject(Long.toString(userPrincipal.getId()))
                .claim("email", userPrincipal.getEmail())
                .claim("role", userPrincipal.getRole())
                .issuedAt(now)
                .expiration(expiryDate)
                .signWith(getSigningKey())
                .compact();
    }

    public boolean validateToken(String authToken) {
        try {
            Jwts.parser()
                    .verifyWith(getSigningKey())
                    .build()
                    .parseSignedClaims(authToken);
            return true;
        } catch (JwtException | IllegalArgumentException e) {
            log.error("Invalid JWT: {}", e.getMessage());
            return false;
        }
    }
}
```

### Password Security

Uses Spring Security's built-in `PasswordEncoder` bean (not a custom class):

```java
@Bean
public PasswordEncoder passwordEncoder() {
    return new BCryptPasswordEncoder();
}
```

Injected in services:

```java
private final PasswordEncoder passwordEncoder;

// Usage
user.setPasswordHash(passwordEncoder.encode(request.getPassword()));
```

## Database Conventions

### Table Naming

- Use **snake_case**
- Use **plural nouns**
- Examples: `users`, `sales_orders`, `inventory_items`, `invoice_lines`

### Column Naming

- Use **snake_case**
- Foreign keys: `<table>_id` (e.g., `user_id`, `role_id`)
- Timestamps: `created_at`, `updated_at`, `deleted_at`
- Booleans: `is_<adjective>` (e.g., `is_active`, `is_deleted`)

### Indexes

```sql
-- Indexes for common queries
CREATE INDEX idx_users_email ON users(email);
CREATE INDEX idx_orders_customer_id ON orders(customer_id);
CREATE INDEX idx_orders_status ON orders(status);

-- Composite indexes
CREATE INDEX idx_orders_customer_status ON orders(customer_id, status);
```

---
*Last audited: 2026-05-10*
