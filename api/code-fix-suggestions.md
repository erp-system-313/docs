# Code Fix Suggestions

## Critical Issues

### 1. Dual API Client (web/src/services/apiClient.ts + web/src/api/client.ts)

**Problem**: Two separate Axios instances with different base URLs:
- `services/apiClient.ts` → baseURL `/api/v1`
- `api/client.ts` → baseURL `/api` (used with `endpoints.ts` paths like `/v1/customers`)

Both resolve to the same backend but create maintenance burden and confusion. The interceptors also differ slightly (one checks `"token"` key, the other doesn't).

**Fix**: Consolidate to a single API client. Recommend keeping `services/apiClient.ts` (baseURL `/api/v1`) and updating all services to use it with direct paths like `/users` instead of going through `endpoints.ts`.

Alternatively, keep `api/client.ts` + `endpoints.ts` pattern and migrate the remaining services (users, auth, audit-log, hr, settings, dashboard) to use it.

### 2. Missing Endpoints in endpoints.ts

**File**: `web/src/api/endpoints.ts`

Missing paths that exist in backend controllers:

| Missing Path | Controller |
|---|---|
| `GET /v1/users/email/{email}` | UserController |
| `GET /v1/attendance/{id}` | AttendanceController |
| `DELETE /v1/attendance/{id}` | AttendanceController |
| `GET /v1/accounts/type/{type}` | AccountController |
| `GET /v1/leave-requests/balances` | LeaveController |

**Fix**: Add these paths to the corresponding endpoint objects.

### 3. Invoice Entity: duplicate `dueDate` / `dueAt` fields

**File**: `service/src/main/java/com/erp/finance/entity/Invoice.java`

```
@Column(name = "due_date")
private LocalDateTime dueDate;

@Column(name = "due_at")
private LocalDateTime dueAt;
```

Both fields appear to serve the same purpose. If `dueAt` was intended as a replacement, remove `dueDate`. If they track different things, add documentation clarifying the difference.

### 4. Inconsistent Error Silencing Pattern in Frontend

**Files**: `web/src/services/financeService.ts`, `web/src/services/salesService.ts`, `web/src/services/purchasingService.ts`

Many service methods catch errors and return null/default values instead of propagating errors:

```typescript
try {
  // ...
} catch (error) {
  console.error("Failed to fetch invoices:", error);
  return { items: [], total: 0 };
}
```

This makes it impossible for UI components to distinguish between "no data" and "server error". Consider re-throwing or returning a discriminated union type.

---

## Medium Priority

### 5. Frontend Type Redundancy

**Files**: `web/src/types/finance.ts`, `web/src/services/financeService.ts`

The service file re-declares `ApiResponse<T>` and `PageResponse<T>` interfaces locally instead of importing from shared types. These duplicate the backend contracts and will drift over time.

**Fix**: Define shared API response types once (e.g., in `types/api.ts`) and import everywhere.

### 6. `console.error` vs Logger

Frontend uses `console.error` directly in service files instead of a centralized logging utility. The docs mention `no-console` ESLint warning but it's not enforced.

**Fix**: Create a simple logger utility and ban `console.*` calls via ESLint.

### 7. Inventory Purchasing Pages Use Flat Structure While Others Use Directory Structure

**Inconsistent pattern**: 
- `pages/inventory/CategoryListPage.tsx` — flat file
- `pages/finance/InvoicesList/` — directory with CSS module + index.ts

**Fix**: Normalize to one pattern. Directory pattern is preferred (barrel exports with co-located CSS modules).

---

## Low Priority

### 8. `services/apiClient.ts` Re-exports `api/client.ts`

Line 86 of `services/apiClient.ts`:
```typescript
export { apiClient } from "../api/client";
```

This creates a circular-looking dependency. Either consolidate (see #1) or remove this re-export.

### 9. Missing `@Transactional` on Some Service Methods

Brief scan found some service methods that perform writes (save/delete) but lack `@Transactional` annotation. Review all service write methods and add `@Transactional` where missing.
