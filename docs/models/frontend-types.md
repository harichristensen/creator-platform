# Frontend Models and Types

## User Types

```typescript
// User authentication and profile types
export interface User {
  id: string;
  email: string;
  username: string;
  role: UserRole;
  profileImageUrl?: string;
  coverImageUrl?: string;
  bio?: string;
  createdAt: string;
  updatedAt: string;
}

export enum UserRole {
  USER = 'USER',
  CREATOR = 'CREATOR',
  ADMIN = 'ADMIN'
}

export interface AuthResponse {
  user: User;
  accessToken: string;
  refreshToken: string;
}

export interface LoginRequest {
  email: string;
  password: string;
}

export interface RegisterRequest {
  email: string;
  username: string;
  password: string;
}
```

## Creator Types

```typescript
export interface Creator {
  id: string;
  userId: string;
  displayName: string;
  description?: string;
  profileImageUrl?: string;
  coverImageUrl?: string;
  subscriptionPrice: number;
  cryptoAddress?: string;
  categories: Category[];
  mediaCount: number;
  subscriberCount: number;
  createdAt: string;
  updatedAt: string;
}

export interface CreatorProfile extends Creator {
  user: User;
  recentMedia: Media[];
  isSubscribed?: boolean;
}
```

## Subscription Types

```typescript
export interface Subscription {
  id: string;
  userId: string;
  creatorId: string;
  creator?: Creator;
  isActive: boolean;
  startDate: string;
  endDate?: string;
  autoRenew: boolean;
  lastPaymentDate?: string;
  nextPaymentDate?: string;
}

export interface SubscriptionRequest {
  creatorId: string;
  paymentMethodId: string;
  paymentType: PaymentMethod;
}
```

## Media Types

```typescript
export interface Media {
  id: string;
  creatorId: string;
  filename: string;
  mimeType: string;
  size: number;
  url: string;
  thumbnailUrl?: string;
  isPremium: boolean;
  title?: string;
  description?: string;
  createdAt: string;
  hasAccess?: boolean;
}

export interface MediaUploadRequest {
  file: File;
  title?: string;
  description?: string;
  isPremium: boolean;
}
```

## Payment Types

```typescript
export enum PaymentMethod {
  STRIPE = 'STRIPE',
  CRYPTO = 'CRYPTO'
}

export interface Transaction {
  id: string;
  userId: string;
  amount: number;
  currency: string;
  paymentMethod: PaymentMethod;
  stripeId?: string;
  cryptoTxHash?: string;
  status: string;
  description?: string;
  createdAt: string;
}

export interface PaymentIntent {
  clientSecret: string;
  amount: number;
  currency: string;
}
```

## Support Types

```typescript
export enum TicketStatus {
  OPEN = 'OPEN',
  IN_PROGRESS = 'IN_PROGRESS',
  RESOLVED = 'RESOLVED',
  CLOSED = 'CLOSED'
}

export interface SupportTicket {
  id: string;
  userId: string;
  user?: User;
  subject: string;
  description: string;
  status: TicketStatus;
  priority: string;
  createdAt: string;
  updatedAt: string;
  resolvedAt?: string;
}

export interface CreateTicketRequest {
  subject: string;
  description: string;
  priority?: 'low' | 'normal' | 'high' | 'urgent';
}
```

## Category Types

```typescript
export interface Category {
  id: string;
  name: string;
  slug: string;
  description?: string;
}
```

## API Response Types

```typescript
export interface ApiResponse<T> {
  success: boolean;
  data?: T;
  error?: string;
  message?: string;
}

export interface PaginatedResponse<T> {
  items: T[];
  total: number;
  page: number;
  pageSize: number;
  hasMore: boolean;
}

export interface ErrorResponse {
  error: string;
  message: string;
  statusCode: number;
  timestamp: string;
}
```

## Form Types

```typescript
export interface ProfileUpdateRequest {
  username?: string;
  bio?: string;
  profileImage?: File;
  coverImage?: File;
}

export interface CreatorApplicationRequest {
  displayName: string;
  description: string;
  subscriptionPrice: number;
  categories: string[];
  cryptoAddress?: string;
}

export interface MediaFilterParams {
  creatorId?: string;
  isPremium?: boolean;
  categoryId?: string;
  page?: number;
  limit?: number;
  sortBy?: 'newest' | 'oldest' | 'popular';
}
```

## Context Types

```typescript
export interface AuthContextType {
  user: User | null;
  isLoading: boolean;
  isAuthenticated: boolean;
  login: (credentials: LoginRequest) => Promise<void>;
  register: (data: RegisterRequest) => Promise<void>;
  logout: () => void;
  refreshToken: () => Promise<void>;
  updateProfile: (data: ProfileUpdateRequest) => Promise<void>;
}

export interface SubscriptionContextType {
  subscriptions: Subscription[];
  isLoading: boolean;
  subscribe: (request: SubscriptionRequest) => Promise<void>;
  unsubscribe: (creatorId: string) => Promise<void>;
  checkAccess: (creatorId: string) => boolean;
  refreshSubscriptions: () => Promise<void>;
}

export interface ThemeContextType {
  isDark: boolean;
  toggleTheme: () => void;
}
```

## Utility Types

```typescript
export type Optional<T, K extends keyof T> = Omit<T, K> & Partial<Pick<T, K>>;

export type AsyncState<T> = {
  data: T | null;
  loading: boolean;
  error: Error | null;
};

export interface ValidationError {
  field: string;
  message: string;
}

export interface UploadProgress {
  loaded: number;
  total: number;
  percentage: number;
}
```

## Hook Return Types

```typescript
export interface UseApiReturn<T> {
  data: T | null;
  loading: boolean;
  error: Error | null;
  refetch: () => Promise<void>;
}

export interface UseInfiniteScrollReturn<T> {
  items: T[];
  loading: boolean;
  hasMore: boolean;
  loadMore: () => Promise<void>;
  reset: () => void;
}

export interface UseDebounceReturn<T> {
  value: T;
  isPending: boolean;
}
```

## Last Updated: 2025-01-13
