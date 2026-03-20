# Backend Guidelines

## Project Structure

```
backend/
├── src/
│   └── main/
│       └── java/
│           └── com/
│               └── erp/
│                   ├── config/          # Configuration classes
│                   │   ├── SecurityConfig.java
│                   │   └── WebConfig.java
│                   ├── controller/      # REST Controllers
│                   ├── service/         # Business logic
│                   ├── repository/      # Data access (JPA)
│                   ├── entity/           # JPA Entities
│                   ├── dto/              # Data Transfer Objects
│                   ├── mapper/          # Entity-DTO mappers
│                   ├── exception/       # Custom exceptions
│                   └── util/            # Utility classes
├── src/
│   └── test/                    # Unit tests
├── resources/
│   ├── application.yml          # Application config
│   └── db/migration/            # Flyway migrations
├── pom.xml                      # Maven dependencies
└── Dockerfile
```

## Package Organization

Each module gets its own package:

```
com.erp.inventory.*
com.erp.sales.*
com.erp.hr.*
com.erp.admin.*
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
| REST Endpoints | kebab-case, plural nouns | `/users`, `/sales-orders` |

## Layer Structure

### 1. Controller Layer

```java
@RestController
@RequestMapping("/api/users")
@RequiredArgsConstructor
@Slf4j
public class UserController {
    
    private final UserService userService;
    
    @GetMapping
    public ResponseEntity<ApiResponse<Page<UserDto>>> getAllUsers(
            @RequestParam(defaultValue = "0") int page,
            @RequestParam(defaultValue = "20") int size) {
        
        log.info("Fetching all users - page: {}, size: {}", page, size);
        Page<UserDto> users = userService.findAll(page, size);
        return ResponseEntity.ok(ApiResponse.success(users));
    }
    
    @GetMapping("/{id}")
    public ResponseEntity<ApiResponse<UserDto>> getUserById(@PathVariable Long id) {
        return userService.findById(id)
                .map(user -> ResponseEntity.ok(ApiResponse.success(user)))
                .orElseThrow(() -> new ResourceNotFoundException("User", id));
    }
    
    @PostMapping
    public ResponseEntity<ApiResponse<UserDto>> createUser(
            @Valid @RequestBody CreateUserRequest request) {
        
        log.info("Creating new user with email: {}", request.getEmail());
        UserDto created = userService.create(request);
        return ResponseEntity.status(HttpStatus.CREATED)
                .body(ApiResponse.success(created));
    }
    
    @PutMapping("/{id}")
    public ResponseEntity<ApiResponse<UserDto>> updateUser(
            @PathVariable Long id,
            @Valid @RequestBody UpdateUserRequest request) {
        
        UserDto updated = userService.update(id, request);
        return ResponseEntity.ok(ApiResponse.success(updated));
    }
    
    @DeleteMapping("/{id}")
    public ResponseEntity<Void> deleteUser(@PathVariable Long id) {
        userService.delete(id);
        return ResponseEntity.noContent().build();
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
    private final UserMapper userMapper;
    
    public Page<UserDto> findAll(int page, int size) {
        Pageable pageable = PageRequest.of(page, size, Sort.by("createdAt").descending());
        return userRepository.findAll(pageable).map(userMapper::toDto);
    }
    
    public Optional<UserDto> findById(Long id) {
        return userRepository.findById(id).map(userMapper::toDto);
    }
    
    public UserDto create(CreateUserRequest request) {
        if (userRepository.existsByEmail(request.getEmail())) {
            throw new BusinessException("USER_001", "Email already exists");
        }
        
        User user = userMapper.toEntity(request);
        user = userRepository.save(user);
        log.info("Created user with id: {}", user.getId());
        
        return userMapper.toDto(user);
    }
    
    public UserDto update(Long id, UpdateUserRequest request) {
        User user = userRepository.findById(id)
                .orElseThrow(() -> new ResourceNotFoundException("User", id));
        
        userMapper.updateFromRequest(user, request);
        user = userRepository.save(user);
        
        return userMapper.toDto(user);
    }
    
    public void delete(Long id) {
        if (!userRepository.existsById(id)) {
            throw new ResourceNotFoundException("User", id);
        }
        userRepository.deleteById(id);
        log.info("Deleted user with id: {}", id);
    }
}
```

### 3. Repository Layer

```java
@Repository
public interface UserRepository extends JpaRepository<User, Long> {
    
    Optional<User> findByEmail(String email);
    
    boolean existsByEmail(String email);
    
    @Query("SELECT u FROM User u WHERE u.isActive = true AND u.role = :role")
    List<User> findActiveUsersByRole(@Param("role") Role role);
    
    @Query("SELECT COUNT(u) FROM User u WHERE u.createdAt >= :date")
    long countUsersCreatedAfter(@Param("date") LocalDateTime date);
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
    
    @Column(nullable = false, length = 100)
    private String name;
    
    @Column(nullable = false, unique = true)
    private String email;
    
    @Column(nullable = false)
    private String passwordHash;
    
    @Enumerated(EnumType.STRING)
    @Column(nullable = false)
    private UserRole role;
    
    @Column(name = "is_active", nullable = false)
    @Builder.Default
    private boolean isActive = true;
    
    @Column(name = "created_at", nullable = false, updatable = false)
    private LocalDateTime createdAt;
    
    @Column(name = "updated_at")
    private LocalDateTime updatedAt;
    
    @PrePersist
    protected void onCreate() {
        createdAt = LocalDateTime.now();
        updatedAt = LocalDateTime.now();
    }
    
    @PreUpdate
    protected void onUpdate() {
        updatedAt = LocalDateTime.now();
    }
}
```

## DTO Pattern

### Request DTO

```java
@Getter
@Setter
@NoArgsConstructor
@AllArgsConstructor
@Builder
public class CreateUserRequest {
    
    @NotBlank(message = "Name is required")
    @Size(min = 2, max = 100)
    private String name;
    
    @NotBlank(message = "Email is required")
    @Email(message = "Invalid email format")
    private String email;
    
    @NotBlank(message = "Password is required")
    @Size(min = 8, message = "Password must be at least 8 characters")
    private String password;
    
    @NotNull(message = "Role is required")
    private UserRole role;
}
```

### Response DTO

```java
@Getter
@Setter
@NoArgsConstructor
@AllArgsConstructor
@Builder
public class UserDto {
    private Long id;
    private String name;
    private String email;
    private UserRole role;
    private boolean isActive;
    private LocalDateTime createdAt;
}
```

## API Response Format

### Standard Response Wrapper

```java
@Getter
@Setter
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

@Getter
@Setter
@NoArgsConstructor
@AllArgsConstructor
@Builder
public class ErrorInfo {
    private String code;
    private String message;
    private List<FieldError> fieldErrors;
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

### JWT Authentication

```java
@Component
@RequiredArgsConstructor
public class JwtTokenProvider {
    
    private final UserDetailsService userDetailsService;
    
    public String generateToken(Authentication authentication) {
        UserPrincipal userPrincipal = (UserPrincipal) authentication.getPrincipal();
        return Jwts.builder()
                .setSubject(userPrincipal.getId().toString())
                .claim("role", userPrincipal.getRole())
                .setIssuedAt(new Date())
                .setExpiration(new Date(System.currentTimeMillis() + JWT_EXPIRATION))
                .signWith(SignatureAlgorithm.HS512, JWT_SECRET)
                .compact();
    }
    
    public boolean validateToken(String token) {
        try {
            Jwts.parser().setSigningKey(JWT_SECRET).parseClaimsJws(token);
            return true;
        } catch (JwtException | IllegalArgumentException e) {
            return false;
        }
    }
}
```

### Password Security

```java
@Component
@RequiredArgsConstructor
public class PasswordEncoder {
    
    private final BCryptPasswordEncoder encoder = new BCryptPasswordEncoder();
    
    public String encode(String rawPassword) {
        return encoder.encode(rawPassword);
    }
    
    public boolean matches(String rawPassword, String encodedPassword) {
        return encoder.matches(rawPassword, encodedPassword);
    }
}
```

## Database Conventions

### Table Naming

- Use **snake_case**
- Use **plural nouns**
- Examples: `users`, `sales_orders`, `inventory_items`

### Column Naming

- Use **snake_case**
- Foreign keys: `<table>_id` (e.g., `user_id`)
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
