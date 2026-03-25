---
config:
  layout: fixed
---
flowchart LR
 subgraph UseCases["Authentication & user management use cases"]
        UC1["Login"]
        UC2["Logout"]
        UC3["Reset Password"]
        UC4["Enable Two-Factor Auth"]
        UC5["Manage User Accounts"]
        UC6["Assign Roles"]
        UC7["Deactivate User"]
        UC8["Validate Credentials"]
        UC9["Generate JWT Token"]
  end
    Guest["Guest"] --> UC1
    AuthUser["Authenticated User"] --> UC2 & UC3 & UC4
    Admin["Admin"] --> UC5 & UC6 & UC7
    UC1 -.-> UC8 & UC9
    UC5 -.-> UC6