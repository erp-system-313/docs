sequenceDiagram
    actor User
    participant Client
    participant AuthController
    participant AuthService
    participant Database

    User->>Client: Enter username & password
    Client->>AuthController: POST /api/auth/login
    AuthController->>AuthService: login(username, password)
    
    AuthService->>Database: Find user by username
    
    alt Invalid credentials
        Database-->>AuthService: User not found or password mismatch
        AuthService-->>AuthController: Authentication failed
        AuthController-->>Client: 401 Unauthorized
        Client-->>User: Show error message
    else Valid credentials
        Database-->>AuthService: Return user data
        AuthService->>AuthService: Generate JWT token
        AuthService-->>AuthController: Return token + user data
        AuthController-->>Client: 200 OK with token
        Client->>Client: Store token
        Client-->>User: Redirect to dashboard
    end

    Note over Client,Database: Subsequent requests
    Client->>AuthController: GET /api/data (with token)
    AuthController->>AuthService: Validate token
    AuthService-->>AuthController: Valid/Invalid
    AuthController-->>Client: Return data or 401