# Backend Models and Types

## Database Models (Prisma Schema)

```typescript
// Generated Prisma Client Types
import { Prisma, PrismaClient } from '@prisma/client';

export enum UserRole {
  USER = 'USER',
  CREATOR = 'CREATOR',
  ADMIN = 'ADMIN'
}

export enum PaymentMethod {
  STRIPE = 'STRIPE',
  CRYPTO = 'CRYPTO'
}

export enum TicketStatus {
  OPEN = 'OPEN',
  IN_PROGRESS = 'IN_PROGRESS',
  RESOLVED = 'RESOLVED',
  CLOSED = 'CLOSED'
}
```

## Request/Response DTOs

```typescript
// Authentication DTOs
export interface RegisterDTO {
  email: string;
  username: string;
  password: string;
}

export interface LoginDTO {
  email: string;
  password: string;
}

export interface TokenPayload {
  id: string;
  email: string;
  role: UserRole;
}

export interface AuthResponseDTO {
  user: UserResponseDTO;
  accessToken: string;
  refreshToken: string;
}

export interface RefreshTokenDTO {
  refreshToken: string;
}

// User DTOs
export interface UserResponseDTO {
  id: string;
  email: string;
  username: string;
  role: UserRole;
  profileImageUrl?: string;
  coverImageUrl?: string;
  bio?: string;
  createdAt: Date;
  updatedAt: Date;
}

export interface UpdateUserDTO {
  username?: string;
  bio?: string;
  profileImageUrl?: string;
  coverImageUrl?: string;
}

export interface ChangePasswordDTO {
  currentPassword: string;
  newPassword: string;
}

// Creator DTOs
export interface CreateCreatorDTO {
  displayName: string;
  description?: string;
  subscriptionPrice: number;
  cryptoAddress?: string;
  categories?: string[];
}

export interface UpdateCreatorDTO {
  displayName?: string;
  description?: string;
  subscriptionPrice?: number;
  cryptoAddress?: string;
  profileImageUrl?: string;
  coverImageUrl?: string;
  categories?: string[];
}

export interface CreatorResponseDTO {
  id: string;
  userId: string;
  displayName: string;
  description?: string;
  profileImageUrl?: string;
  coverImageUrl?: string;
  subscriptionPrice: number;
  cryptoAddress?: string;
  subscriberCount?: number;
  mediaCount?: number;
  categories?: CategoryDTO[];
  createdAt: Date;
  updatedAt: Date;
}

// Subscription DTOs
export interface CreateSubscriptionDTO {
  creatorId: string;
  paymentMethodId?: string;
  paymentType: PaymentMethod;
}

export interface SubscriptionResponseDTO {
  id: string;
  userId: string;
  creatorId: string;
  creator?: CreatorResponseDTO;
  isActive: boolean;
  startDate: Date;
  endDate?: Date;
  autoRenew: boolean;
  lastPaymentDate?: Date;
  nextPaymentDate?: Date;
}

export interface UpdateSubscriptionDTO {
  autoRenew?: boolean;
}

// Media DTOs
export interface CreateMediaDTO {
  creatorId: string;
  filename: string;
  mimeType: string;
  size: number;
  url: string;
  thumbnailUrl?: string;
  isPremium: boolean;
  title?: string;
  description?: string;
}

export interface MediaResponseDTO {
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
  createdAt: Date;
  hasAccess?: boolean;
}

export interface MediaUploadDTO {
  file: Express.Multer.File;
  title?: string;
  description?: string;
  isPremium: boolean;
}

// Payment DTOs
export interface CreatePaymentIntentDTO {
  amount: number;
  currency: string;
  creatorId: string;
  subscriptionId?: string;
}

export interface PaymentIntentResponseDTO {
  clientSecret: string;
  amount: number;
  currency: string;
}

export interface ProcessCryptoPaymentDTO {
  creatorId: string;
  txHash: string;
  amount: number;
  currency: string;
}

export interface TransactionResponseDTO {
  id: string;
  userId: string;
  amount: number;
  currency: string;
  paymentMethod: PaymentMethod;
  stripeId?: string;
  cryptoTxHash?: string;
  status: string;
  description?: string;
  createdAt: Date;
}

// Support Ticket DTOs
export interface CreateTicketDTO {
  subject: string;
  description: string;
  priority?: 'low' | 'normal' | 'high' | 'urgent';
}

export interface UpdateTicketDTO {
  status?: TicketStatus;
  priority?: string;
  description?: string;
}

export interface TicketResponseDTO {
  id: string;
  userId: string;
  user?: UserResponseDTO;
  subject: string;
  description: string;
  status: TicketStatus;
  priority: string;
  createdAt: Date;
  updatedAt: Date;
  resolvedAt?: Date;
}

// Category DTOs
export interface CategoryDTO {
  id: string;
  name: string;
  slug: string;
  description?: string;
}

export interface CreateCategoryDTO {
  name: string;
  slug: string;
  description?: string;
}

// Admin DTOs
export interface AdminStatsDTO {
  totalUsers: number;
  totalCreators: number;
  totalSubscriptions: number;
  activeSubscriptions: number;
  totalRevenue: number;
  revenueByMonth: { month: string; amount: number }[];
  topCreators: CreatorResponseDTO[];
  recentTransactions: TransactionResponseDTO[];
  openTickets: number;
}

export interface AdminUserListDTO {
  users: UserResponseDTO[];
  total: number;
  page: number;
  pageSize: number;
}

export interface AdminContentModerationDTO {
  mediaId: string;
  action: 'approve' | 'reject' | 'flag';
  reason?: string;
}
```

## Service Layer Types

```typescript
// Service Response Types
export interface ServiceResponse<T> {
  success: boolean;
  data?: T;
  error?: string;
}

export interface PaginationParams {
  page: number;
  limit: number;
  sortBy?: string;
  sortOrder?: 'asc' | 'desc';
}

export interface PaginatedResult<T> {
  items: T[];
  total: number;
  page: number;
  pageSize: number;
  totalPages: number;
}

// File Upload Types
export interface FileUploadResult {
  url: string;
  thumbnailUrl?: string;
  size: number;
  mimeType: string;
  filename: string;
}

export interface StorageService {
  upload(file: Express.Multer.File, path: string): Promise<FileUploadResult>;
  delete(url: string): Promise<void>;
  getSignedUrl(url: string, expiresIn?: number): Promise<string>;
}

// Email Service Types
export interface EmailOptions {
  to: string;
  subject: string;
  html: string;
  text?: string;
}

export interface EmailTemplate {
  welcome: (user: UserResponseDTO) => EmailOptions;
  subscriptionConfirmation: (subscription: SubscriptionResponseDTO) => EmailOptions;
  ticketCreated: (ticket: TicketResponseDTO) => EmailOptions;
  ticketResolved: (ticket: TicketResponseDTO) => EmailOptions;
  passwordReset: (user: UserResponseDTO, token: string) => EmailOptions;
}

// Webhook Types
export interface StripeWebhookEvent {
  type: string;
  data: {
    object: any;
  };
}

export interface CryptoWebhookEvent {
  txHash: string;
  from: string;
  to: string;
  amount: string;
  confirmations: number;
  status: 'pending' | 'confirmed' | 'failed';
}
```

## Middleware Types

```typescript
// Request Extensions
export interface AuthenticatedRequest extends Express.Request {
  user?: TokenPayload;
  file?: Express.Multer.File;
  files?: Express.Multer.File[];
}

export interface RateLimitOptions {
  windowMs: number;
  max: number;
  message?: string;
  skipSuccessfulRequests?: boolean;
}

// Error Types
export class AppError extends Error {
  statusCode: number;
  isOperational: boolean;

  constructor(message: string, statusCode: number) {
    super(message);
    this.statusCode = statusCode;
    this.isOperational = true;
    Error.captureStackTrace(this, this.constructor);
  }
}

export class ValidationError extends AppError {
  fields: { [key: string]: string };

  constructor(fields: { [key: string]: string }) {
    super('Validation failed', 400);
    this.fields = fields;
  }
}

export class AuthenticationError extends AppError {
  constructor(message: string = 'Authentication failed') {
    super(message, 401);
  }
}

export class AuthorizationError extends AppError {
  constructor(message: string = 'Insufficient permissions') {
    super(message, 403);
  }
}

export class NotFoundError extends AppError {
  constructor(resource: string) {
    super(`${resource} not found`, 404);
  }
}
```

## Configuration Types

```typescript
// Environment Configuration
export interface Config {
  env: 'development' | 'staging' | 'production';
  port: number;
  database: {
    url: string;
    maxConnections: number;
  };
  jwt: {
    secret: string;
    refreshSecret: string;
    expiresIn: string;
    refreshExpiresIn: string;
  };
  stripe: {
    secretKey: string;
    webhookSecret: string;
    publicKey?: string;
  };
  crypto: {
    blockchairApiKey: string;
    supportedCurrencies: string[];
  };
  storage: {
    provider: 'azure' | 'minio' | 'local';
    connectionString: string;
    containerName: string;
  };
  email: {
    host: string;
    port: number;
    user: string;
    pass: string;
    from: string;
  };
  sentry: {
    dsn: string;
    environment: string;
  };
  cors: {
    origin: string | string[];
    credentials: boolean;
  };
  rateLimit: {
    general: RateLimitOptions;
    auth: RateLimitOptions;
    api: RateLimitOptions;
  };
}
```

## Validation Schemas

```typescript
// Using Joi or Zod for validation
import * as z from 'zod';

export const RegisterSchema = z.object({
  email: z.string().email(),
  username: z.string().min(3).max(30).regex(/^[a-zA-Z0-9_]+$/),
  password: z.string().min(8).max(100),
});

export const LoginSchema = z.object({
  email: z.string().email(),
  password: z.string(),
});

export const CreateCreatorSchema = z.object({
  displayName: z.string().min(1).max(100),
  description: z.string().max(500).optional(),
  subscriptionPrice: z.number().positive(),
  cryptoAddress: z.string().optional(),
  categories: z.array(z.string()).optional(),
});

export const CreateTicketSchema = z.object({
  subject: z.string().min(1).max(200),
  description: z.string().min(1).max(2000),
  priority: z.enum(['low', 'normal', 'high', 'urgent']).optional(),
});

export const PaginationSchema = z.object({
  page: z.number().int().positive().default(1),
  limit: z.number().int().positive().max(100).default(20),
  sortBy: z.string().optional(),
  sortOrder: z.enum(['asc', 'desc']).optional(),
});
```

## Repository Types

```typescript
// Base Repository Interface
export interface BaseRepository<T> {
  findById(id: string): Promise<T | null>;
  findAll(params?: PaginationParams): Promise<PaginatedResult<T>>;
  create(data: Partial<T>): Promise<T>;
  update(id: string, data: Partial<T>): Promise<T>;
  delete(id: string): Promise<void>;
}

// Specific Repository Interfaces
export interface UserRepository extends BaseRepository<User> {
  findByEmail(email: string): Promise<User | null>;
  findByUsername(username: string): Promise<User | null>;
  updateRole(userId: string, role: UserRole): Promise<User>;
}

export interface CreatorRepository extends BaseRepository<Creator> {
  findByUserId(userId: string): Promise<Creator | null>;
  findByCategory(categoryId: string, params?: PaginationParams): Promise<PaginatedResult<Creator>>;
  updateSubscriberCount(creatorId: string, delta: number): Promise<void>;
}

export interface SubscriptionRepository extends BaseRepository<Subscription> {
  findByUserAndCreator(userId: string, creatorId: string): Promise<Subscription | null>;
  findActiveByUser(userId: string): Promise<Subscription[]>;
  findExpiring(days: number): Promise<Subscription[]>;
  updateStatus(id: string, isActive: boolean): Promise<Subscription>;
}

export interface MediaRepository extends BaseRepository<Media> {
  findByCreator(creatorId: string, params?: PaginationParams): Promise<PaginatedResult<Media>>;
  findPremiumByCreator(creatorId: string, params?: PaginationParams): Promise<PaginatedResult<Media>>;
  checkAccess(mediaId: string, userId: string): Promise<boolean>;
}
```

## Last Updated: 2025-01-13
