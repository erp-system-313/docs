# Backend Development Setup

## Prerequisites

| Tool | Version | Purpose |
|------|---------|---------|
| JDK | 21 | Java development |
| Docker | Latest | Container runtime |
| Docker Compose | Latest | Database services |
| Maven | 3.9+ | Build tool |
| Nix | 2.18+ (with flakes) | Dev environment (optional) |

---

## Quick Start

```bash
# 1. Clone the service repository
git clone <repository-url>
cd service

# 2. Start database services
docker-compose up -d

# 3. Enter development environment
nix develop

# 4. Build the project
mvn clean install -DskipTests

# 5. Run the application
mvn spring-boot:run
```

The API will be available at **http://localhost:8080**

---

## Development Environment (Nix Flakes)

The project includes a `flake.nix` that provides a reproducible development shell.

### Entering the Dev Shell

```bash
nix develop
```

This provides:
- JDK 21
- Maven 3.9+
- Git

### Rebuilding the Shell

If you modify `flake.nix`, rebuild with:

```bash
nix develop --rebuild
```

### Without Nix

If you prefer not to use Nix, ensure the following are installed manually:
- JDK 21
- Maven 3.9+

---

## Maven Commands

```bash
# Compile the project
mvn clean compile

# Build without running tests
mvn clean install -DskipTests

# Run the application
mvn spring-boot:run

# Run tests
mvn test

# Run a specific test class
mvn test -Dtest=ErpApplicationTests

# Clean build artifacts
mvn clean
```

---

## Database Services (Docker Compose)

The `docker-compose.yml` provides PostgreSQL and Redis for local development.

### Starting Services

```bash
docker-compose up -d
```

### Stopping Services

```bash
docker-compose down
```

### Service Details

| Service | Port | Default Credentials |
|---------|------|---------------------|
| PostgreSQL | 5432 | user: `erp`, password: `erp`, db: `erp` |
| Redis | 6379 | (no authentication) |

### Health Checks

```bash
# Check container status
docker-compose ps

# View logs
docker-compose logs -f postgres
```

### Resetting the Database

```bash
# Stop and remove volumes
docker-compose down -v

# Start fresh
docker-compose up -d
```

---

## Accessing the Application

| Endpoint | URL |
|----------|-----|
| API Base | http://localhost:8080 |
| Swagger UI | http://localhost:8080/swagger-ui.html |
| OpenAPI JSON | http://localhost:8080/api-docs |

---

## Configuration

Configuration is in `src/main/resources/application.yml`.

### Key Settings

```yaml
server:
  port: 8080

spring:
  datasource:
    url: jdbc:postgresql://localhost:5432/erp
    username: erp
    password: erp

  data:
    redis:
      host: localhost
      port: 6379

jwt:
  secret: ${JWT_SECRET:your-256-bit-secret-key-here-must-be-at-least-32-chars}
```

### Environment Variables

Override defaults by setting environment variables:

```bash
export JWT_SECRET=your-secret-key
export SPRING_DATASOURCE_PASSWORD=your-db-password
mvn spring-boot:run
```

---

## Common Issues

### Port Already in Use

```bash
# Find process using port 8080
lsof -i :8080

# Kill it or change port in application.yml
```

### Database Connection Failed

```bash
# Verify PostgreSQL is running
docker-compose ps

# Check logs
docker-compose logs postgres
```

### Maven Wrapper Not Available

```bash
# Install Maven or use nix develop which includes it
mvn --version
```
