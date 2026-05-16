sequenceDiagram
    actor Employee
    actor Manager
    participant LeaveController
    participant LeaveService
    participant Database

    %% Employee submits leave
    Employee->>LeaveController: POST /api/leave-requests
    LeaveController->>LeaveService: submitRequest(data)
    
    LeaveService->>Database: Check leave balance
    Database-->>LeaveService: Balance info
    
    alt Insufficient balance
        LeaveService-->>LeaveController: Error
        LeaveController-->>Employee: 400 - Insufficient balance
    else Balance OK
        LeaveService->>Database: Save leave request (status: PENDING)
        Database-->>LeaveService: Request saved
        LeaveService-->>LeaveController: Success
        LeaveController-->>Employee: 201 - Request submitted
    end

    %% Manager approves/rejects
    Manager->>LeaveController: GET /api/leave-requests?status=PENDING
    LeaveController->>LeaveService: getPendingRequests()
    LeaveService->>Database: Query pending requests
    Database-->>LeaveService: Return list
    LeaveController-->>Manager: Display pending requests
    
    Manager->>LeaveController: POST /api/leave-requests/{id}/approve
    LeaveController->>LeaveService: approveRequest(id)
    LeaveService->>Database: Update status to APPROVED
    LeaveService->>Database: Deduct leave balance
    Database-->>LeaveService: Updated
    LeaveService-->>LeaveController: Success
    LeaveController-->>Manager: Approval confirmed
    
    opt Rejection instead
        Manager->>LeaveController: POST /api/leave-requests/{id}/reject
        LeaveController->>LeaveService: rejectRequest(id)
        LeaveService->>Database: Update status to REJECTED
        LeaveService-->>LeaveController: Success
        LeaveController-->>Manager: Rejection confirmed
    end
    
    LeaveController-->>Employee: Email/Notification about decision