## 1. NavigationShell

| Component | Purpose |
|-----------|---------|
| **NavigationShell** | Root layout container providing consistent navigation structure across all ERP modules. Manages sidebar state and top bar coordination. |
| **Sidebar** | Collapsible navigation panel containing module links and collapse controls. |
| ↳ NavItems | List of navigation links organized by module access (Inventory, Sales, HR, Admin). |
| ↳ CollapseButton | Toggles sidebar expanded/collapsed state, persists preference in localStorage. |
| **TopBar** | Fixed header bar containing global tools and user controls. |
| ↳ GlobalSearch | Universal search input querying across all modules (products, customers, employees). |
| ↳ NotificationBell | Dropdown displaying real-time system alerts and approval requests. |
| ↳ UserProfileMenu | Dropdown menu with profile navigation, settings access, and logout. |
| ↳ ProfileLink | Navigates to self-service profile view. |
| ↳ SettingsLink | Navigates to system settings (admin only) or personal preferences. |
| ↳ LogoutButton | Clears JWT token, redirects to login page. |

---

## 2. DashboardWidget

| Component | Purpose |
|-----------|---------|
| **DashboardWidget** | Top-level container for the ERP dashboard. Aggregates KPIs, charts, and quick actions in a responsive grid layout. |
| **StatsRow** | Horizontal row displaying key performance indicator cards. |
| ↳ RevenueCard | Displays total revenue with trend indicator (up/down percentage). |
| ↳ InventoryCard | Shows low stock alerts and total SKU count. |
| ↳ PendingApprovalsCard | Counter of pending leave requests, purchase orders awaiting approval. |
| **ChartsRow** | Container for data visualization widgets. |
| ↳ RevenueLineChart | Time-series chart of revenue over selected period (week/month/quarter). |
| ↳ InventoryBarChart | Category breakdown of current stock levels. |
| **ActivityRow** | Bottom section combining recent events and shortcuts. |
| ↳ RecentActivityList | Feed of system events (new orders, approved leaves, stock updates) with timestamps. |
| ↳ QuickActionsPanel | Grid of shortcut buttons for common tasks. |
| ↳ NewOrderButton | Direct navigation to sales order creation form. |
| ↳ AddEmployeeButton | Direct navigation to employee onboarding form. |

---

## 3. UserManagement

| Component | Purpose |
|-----------|---------|
| **UserManagement** | Main container for system user administration. Provides user CRUD operations and role assignment. |
| **PageHeader** | Top section with title and primary action. |
| ↳ Title | "User Management" heading with user count badge. |
| ↳ AddUserButton | Opens modal/form for creating new system user, sends email invitation. |
| **UserManagementContent** | Main content area with filtering and data display. |
| ↳ FilterBar | Horizontal toolbar for refining user list. |
| ↳ SearchInput | Text search across name, email, employee ID fields. |
| ↳ RoleDropdown | Filter by assigned role (Admin, HR Manager, Sales, etc.). |
| ↳ StatusDropdown | Filter by account status (Active, Inactive, Pending). |
| ↳ UsersDataGrid | Paginated table displaying user records. |
| ↳ UserRow | Individual row representing one user account. |
| ↳ UserAvatar | Profile image thumbnail (fallback to initials). |
| ↳ UserName | Full name with link to detail view. |
| ↳ UserEmail | Email address with mailto link. |
| ↳ RoleBadge | Visual badge indicating primary role (color-coded). |
| ↳ ActionButtons | Row-level operations requiring confirmation where destructive. |
| ↳ EditButton | Opens user edit modal (name, email, role changes). |
| ↳ DeleteButton | Soft delete with confirmation dialog, checks for active sessions. |
| ↳ ResetPasswordButton | Triggers password reset email to user. |
| ↳ Pagination | Page controls and items-per-page selector. |

---

## 4. CompanySettings

| Component | Purpose |
|-----------|---------|
| **CompanySettings** | Configuration hub for organization-wide settings and structure management. |
| **SettingsTabs** | Tab container organizing settings into logical groups. |
| ↳ GeneralTab | Company identity and legal information. |
| ↳ LogoUpload | Drag-and-drop image upload with preview, validates file type/size. |
| ↳ CompanyNameInput | Legal company name (appears on invoices, reports). |
| ↳ TaxIdInput | Tax identification number field with validation format. |
| ↳ AddressFields | Multi-line address input (street, city, postal, country). |
| ↳ SaveButton | Persists general settings, shows success toast on completion. |
| ↳ DepartmentsTab | Organizational structure management. |
| ↳ DepartmentList | Vertical list of configured departments. |
| ↳ DepartmentItem | Single department row with inline editing. |
| ↳ DepartmentName | Editable text field (e.g., "Engineering", "Sales"). |
| ↳ DepartmentCode | Short code for reporting (e.g., "ENG", "SAL"). |
| ↳ DeptActions | Edit and delete controls per department. |
| ↳ EditButton | Inline edit mode for department fields. |
| ↳ DeleteButton | Remove department (blocked if employees assigned). |
| ↳ NotificationsTab | System notification configuration. |
| ↳ EmailConfig | SMTP server settings, sender address, templates. |
| ↳ SmsConfig | SMS gateway integration settings (optional). |
| ↳ TemplateList | Manageable list of email templates (welcome, password reset, leave approval). |

---

## 5. Profile

| Component | Purpose |
|-----------|---------|
| **Profile** | Self-service interface for employees to manage their own profile and security settings. |
| **ProfileCard** | Personal information section with avatar and contact details. |
| ↳ AvatarSection | Profile photo management area. |
| ↳ ProfileImage | Circular cropped photo display. |
| ↳ ChangePhotoButton | Upload new image with crop/resize preview. |
| ↳ PersonalInfo | Editable contact information form. |
| ↳ FullNameInput | Display name (may differ from system username). |
| ↳ EmailInput | Primary email with verification status indicator. |
| ↳ PhoneInput | Mobile number for SMS notifications (optional). |
| ↳ UpdateButton | Saves profile changes, triggers validation. |
| **SecurityCard** | Account security and session management. |
| ↳ PasswordSection | Password change workflow. |
| ↳ CurrentPassword | Existing password verification (required). |
| ↳ NewPassword | New password with strength meter. |
| ↳ ConfirmPassword | Must match new password field. |
| ↳ ChangePasswordButton | Validates and updates password hash. |
| ↳ TwoFASection | Two-factor authentication management. |
| ↳ StatusBadge | Shows "Enabled" or "Disabled" state. |
| ↳ EnableDisableButton | Toggles 2FA setup modal or removes existing 2FA. |
| ↳ ActiveSessions | Security monitoring of logged-in devices. |
| ↳ SessionList | Table of devices, locations, last active times. |
| ↳ RevokeButton | Terminates specific session (force logout remotely). |

---

## 6. EmployeeDetails

| Component | Purpose |
|-----------|---------|
| **EmployeeDetails** | Comprehensive single-employee view with full employment history. |
| **EmployeeHeader** | Top banner with identity summary. |
| ↳ LargeAvatar | High-resolution profile photo. |
| ↳ EmployeeSummary | Key identity fields. |
| ↳ EmployeeName | Full legal name with preferred name in parentheses. |
| ↳ EmployeeId | System-generated unique identifier (display-only). |
| ↳ EmploymentStatusBadge | Prominent status indicator (hiring stage). |
| **DetailTabs** | Multi-section information organizer. |
| ↳ PersonalTab | Private contact and emergency information. |
| ↳ ContactInfo | Address, phone, personal email. |
| ↳ EmergencyContact | Name, relationship, phone for emergencies. |
| ↳ EmploymentTab | Job-related details and reporting structure. |
| ↳ PositionInfo | Job title, grade level, employment type (full-time/contract). |
| ↳ ReportingLine | Manager name with link to manager's profile. |
| ↳ HireDate | Start date and length of service calculation. |
| ↳ DocumentsTab | Employment document repository. |
| ↳ DocumentList | Grid of uploaded files with metadata. |
| ↳ DocumentItem | Single file representation. |
| ↳ DocumentName | Filename with extension icon. |
| ↳ DocumentType | Category (Contract, ID, Certificate, Visa). |
| ↳ UploadDate | Timestamp for sorting and filtering. |
| ↳ DownloadDelete | Actions to retrieve or remove file. |
| ↳ PayrollTab | Compensation and banking information (restricted access). |
| ↳ SalaryInfo | Current salary, currency, effective date (HR/Admin only). |
| ↳ BankDetails | Account number, bank name for salary deposit (masked). |

---

## 8. Attendance

| Component | Purpose |
|-----------|---------|
| **Attendance** | Calendar-based attendance tracking with self-service clock functionality. |
| **AttendanceHeader** | Calendar navigation and view controls. |
| ↳ MonthYearSelector | Dropdown or picker for navigating months. |
| ↳ TodayButton | Quick jump to current date. |
| ↳ DayWeekMonthToggle | Switches calendar granularity (day view for detail, month for overview). |
| **CalendarGrid** | Main date-based visualization. |
| ↳ WeekDayHeaders | Column headers (Mon-Sun) with date numbers. |
| ↳ DayCell | Individual date container with attendance data. |
| ↳ DayNumber | Date of month (grayed if outside current month). |
| ↳ ShiftInfo | Assigned shift name or "Off" indicator. |
| ↳ CheckInTime | Recorded clock-in timestamp (color: green=on time, red=late). |
| ↳ CheckOutTime | Recorded clock-out timestamp. |
| ↳ AttendanceStatusIcon | Visual indicator (checkmark, warning, absent, leave). |
| **AttendanceSidebar** | Supplementary information panel. |
| ↳ MonthlySummary | Statistics for selected period. |
| ↳ ClockInOutPanel | Self-service time recording interface. |
| ↳ CurrentTimeDisplay | Live clock showing server time. |
| ↳ ClockInButton | Records start time, captures geolocation (if enabled). |
| ↳ LocationStatus | GPS validation indicator (office perimeter check). |

---

## 9. LeaveRequest

| Component | Purpose |
|-----------|---------|
| **LeaveRequest** | Form interface for submitting and managing leave/absence requests. |
| **FormHeader** | Context header with balance overview. |
| ↳ FormTitle | "New Leave Request" or "Edit Request" heading. |
| ↳ LeaveBalanceCard | Visual summary of available time off. |
| ↳ AnnualLeaveBalance | Days remaining vs. total annual entitlement. |
| ↳ SickLeaveBalance | Medical leave remaining (may be unlimited). |
| ↳ CasualLeaveBalance | Short-term personal leave balance. |
| **LeaveForm** | Main submission form with validation. |
| ↳ DateRangePicker | Start and end date selection with duration calculation. |
| ↳ StartDate | Calendar picker (cannot be past date). |
| ↳ EndDate | Must be equal or after start date. |
| ↳ DurationCalculation | Auto-computed business days (excludes weekends/holidays). |
| ↳ LeaveTypeDropdown | Category selection affecting approval workflow. |
| ↳ AnnualLeave | Standard vacation time (requires advance notice). |
| ↳ SickLeave | Medical absence (may require certificate). |
| ↳ CasualLeave | Short personal absences (limited per month). |
| ↳ UnpaidLeave | Leave without salary deduction (requires approval). |
| ↳ ReasonTextArea | Explanation for absence (required for &gt;3 days or sick leave). |
| ↳ AttachmentUpload | File upload for medical certificates or supporting docs. |
| ↳ FormActions | Submission controls with draft option. |
| ↳ SubmitButton | Validates and sends to manager for approval. |
| ↳ CancelButton | Discards form and returns to directory. |
| ↳ SaveDraftButton | Preserves incomplete form for later completion. |