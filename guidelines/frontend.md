# Frontend Guidelines

## Project Structure

```
frontend/
├── src/
│   ├── assets/           # Static assets (images, fonts)
│   ├── components/       # Reusable UI components
│   │   ├── common/       # Generic components (Button, Modal, Table)
│   │   ├── forms/        # Form-specific components
│   │   └── layout/       # Layout components (Sidebar, Header)
│   ├── pages/            # Page components (route-level)
│   ├── hooks/            # Custom React hooks
│   ├── services/         # API calls and external services
│   ├── store/            # State management (Redux/Context)
│   ├── types/            # TypeScript interfaces and types
│   ├── utils/            # Helper functions
│   ├── styles/           # Global styles and theme
│   └── App.tsx
├── public/
├── tests/
├── package.json
└── tsconfig.json
```

## Naming Conventions

| Type | Convention | Example |
|------|------------|---------|
| Components | PascalCase | `UserProfile.tsx` |
| Hooks | camelCase with `use` prefix | `useAuth.ts` |
| Services | camelCase | `authService.ts` |
| Types/Interfaces | PascalCase | `UserResponse.ts` |
| Utils | camelCase | `formatCurrency.ts` |
| CSS/SCSS | kebab-case | `user-profile.scss` |
| Constants | UPPER_SNAKE_CASE | `API_BASE_URL` |

## Component Guidelines

### Component Structure

```tsx
// 1. Imports (external, then internal)
import React from 'react';
import { Button } from '@/components/common';
import { useUser } from '@/hooks';

// 2. Props interface (when applicable)
interface UserCardProps {
  user: User;
  onEdit: (id: string) => void;
}

// 3. Component definition
export const UserCard: React.FC<UserCardProps> = ({ user, onEdit }) => {
  // 4. Hooks first
  const { loading } = useUser(user.id);
  
  // 5. Local state
  const [isExpanded, setIsExpanded] = useState(false);
  
  // 6. Handler functions
  const handleClick = () => {
    setIsExpanded(!isExpanded);
    onEdit(user.id);
  };
  
  // 7. Early returns
  if (loading) return <Skeleton />;
  
  // 8. Render
  return (
    <div className="user-card">
      <h3>{user.name}</h3>
      <button onClick={handleClick}>Edit</button>
    </div>
  );
};

// 9. Default props (if needed)
UserCard.defaultProps = {
  onEdit: () => {},
};

// 10. Named export
export default UserCard;
```

### Component Rules

- Use **functional components** with hooks (no class components)
- Define **explicit prop types** (interface or type)
- **Avoid `any`** - use `unknown` and type guard when necessary
- **One component per file**
- Keep components **small and focused** (max ~150 lines)
- Extract custom hooks for complex logic

## TypeScript Standards

### Type vs Interface

```typescript
// Use interface for object shapes (extensible)
interface User {
  id: string;
  name: string;
  email: string;
}

// Use type for unions, intersections, primitives
type UserRole = 'admin' | 'manager' | 'staff';
type ApiResponse<T> = SuccessResponse<T> | ErrorResponse;
```

### Generic Types

```typescript
// Good: Generic API response
interface ApiResponse<T> {
  data: T;
  status: number;
  message: string;
}

// Good: Generic list with pagination
interface PaginatedList<T> {
  items: T[];
  total: number;
  page: number;
  pageSize: number;
}
```

### Avoid Any

```typescript
// Bad
const handleData = (data: any) => { ... };

// Good
const handleData = (data: User[]) => { ... };

// If type is truly unknown
const handleUnknown = (data: unknown) => {
  if (isUserData(data)) { ... }
};
```

## API Layer

### Service Structure

```typescript
// services/userService.ts
import api from './apiClient';
import { User, CreateUserDto, UpdateUserDto } from '@/types';

export const userService = {
  getAll: async (params?: QueryParams): Promise<PaginatedList<User>> => {
    const response = await api.get('/users', { params });
    return response.data;
  },
  
  getById: async (id: string): Promise<User> => {
    const response = await api.get(`/users/${id}`);
    return response.data;
  },
  
  create: async (data: CreateUserDto): Promise<User> => {
    const response = await api.post('/users', data);
    return response.data;
  },
  
  update: async (id: string, data: UpdateUserDto): Promise<User> => {
    const response = await api.put(`/users/${id}`, data);
    return response.data;
  },
  
  delete: async (id: string): Promise<void> => {
    await api.delete(`/users/${id}`);
  },
};
```

### Error Handling

```typescript
// Centralized error handling in API client
export class ApiError extends Error {
  constructor(
    public status: number,
    public code: string,
    message: string
  ) {
    super(message);
  }
}

// Usage in components
try {
  const user = await userService.getById(id);
} catch (error) {
  if (error instanceof ApiError && error.status === 404) {
    showToast('User not found', 'error');
  }
}
```

## State Management

### Local vs Global State

```typescript
// Local state: useState
const [isOpen, setIsOpen] = useState(false);

// Global state: Use Context or Redux for:
// - Authentication state
// - User preferences
// - Data shared across multiple pages
// - Real-time updates
```

### Context Pattern

```typescript
// contexts/AuthContext.tsx
interface AuthContextType {
  user: User | null;
  login: (credentials: LoginDto) => Promise<void>;
  logout: () => void;
}

const AuthContext = createContext<AuthContextType | undefined>(undefined);

export const AuthProvider: React.FC = ({ children }) => {
  const [user, setUser] = useState<User | null>(null);
  
  const login = async (credentials: LoginDto) => {
    // ... login logic
  };
  
  const logout = () => {
    setUser(null);
  };
  
  return (
    <AuthContext.Provider value={{ user, login, logout }}>
      {children}
    </AuthContext.Provider>
  );
};

export const useAuth = () => {
  const context = useContext(AuthContext);
  if (!context) throw new Error('useAuth must be used within AuthProvider');
  return context;
};
```

## Forms

### Form Validation Rules

- Validate on blur and submit
- Show errors below inputs
- Disable submit until form is valid
- Clear errors on input change

### Form Structure

```tsx
const UserForm: React.FC<FormProps> = ({ onSubmit }) => {
  const [formData, setFormData] = useState(initialData);
  const [errors, setErrors] = useState<FormErrors>({});
  
  const validate = (): boolean => {
    const newErrors: FormErrors = {};
    if (!formData.email) newErrors.email = 'Email is required';
    if (!formData.email.includes('@')) newErrors.email = 'Invalid email';
    setErrors(newErrors);
    return Object.keys(newErrors).length === 0;
  };
  
  const handleSubmit = (e: React.FormEvent) => {
    e.preventDefault();
    if (validate()) {
      onSubmit(formData);
    }
  };
  
  return (
    <form onSubmit={handleSubmit}>
      <input
        value={formData.email}
        onChange={(e) => setFormData({ ...formData, email: e.target.value })}
      />
      {errors.email && <span className="error">{errors.email}</span>}
      <button type="submit">Submit</button>
    </form>
  );
};
```

## Styling Guidelines

### CSS Approach

- Use **CSS Modules** or **styled-components** for component styles
- Use **global styles** for theme variables and resets
- Keep specificity low; avoid `!important`

### Class Naming (BEM)

```css
/* Block */
.user-card { ... }

/* Element */
.user-card__header { ... }
.user-card__name { ... }

/* Modifier */
.user-card--highlighted { ... }
.user-card__button--disabled { ... }
```

## File Organization

### Import Order

```typescript
// 1. React/core libraries
import React, { useState, useEffect } from 'react';

// 2. External libraries
import { Button, Modal } from 'antd';
import { useForm } from 'react-hook-form';

// 3. Internal absolute imports
import { userService } from '@/services';
import { useAuth } from '@/hooks';
import { User } from '@/types';

// 4. Relative imports (if absolute not configured)
import { formatDate } from '../../utils';
```

## Linting & Formatting

### ESLint Rules (eslint.config.js)

```javascript
module.exports = {
  extends: [
    'eslint:recommended',
    'plugin:react/recommended',
    'plugin:@typescript-eslint/recommended',
  ],
  rules: {
    'react/react-in-jsx-scope': 'off',
    '@typescript-eslint/no-explicit-any': 'warn',
    '@typescript-eslint/explicit-function-return-type': 'off',
    'no-console': ['warn', { allow: ['warn', 'error'] }],
  },
};
```

### Prettier Configuration

```json
{
  "semi": true,
  "singleQuote": true,
  "tabWidth": 2,
  "trailingComma": "es5",
  "printWidth": 100,
  "bracketSpacing": true
}
```
