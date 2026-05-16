---
config:
  layout: dagre
---
flowchart LR
 subgraph UC["HR use cases (attendance, leave requests)"]
        UC1["View Attendance Calendar"]
        UC2["Clock In"]
        UC3["Clock Out"]
        UC4["Submit Leave Request"]
        UC5["Approve Leave Request"]
        UC6["Reject Leave Request"]
        UC7["View Leave Balance"]
        UC8["Generate Attendance Report"]
        UC9["Check Leave Balance"]
        UC10["Escalate to HR Director"]
  end
    Emp(["Employee"]) --> UC1 & UC2 & UC3 & UC4 & UC7
    Mgr(["Department Manager"]) --> UC5 & UC6
    HR(["HR Manager"]) --> UC5 & UC6 & UC8
    UC4 -.-> UC9
    UC5 -.-> UC10

    style Emp fill:#e1f5fe,stroke:#01579b
    style Mgr fill:#e1f5fe,stroke:#01579b
    style HR fill:#e1f5fe,stroke:#01579b