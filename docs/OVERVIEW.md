# Creator Platform - Codebase Overview

## Project Structure
```
creator-platform/
├── frontend/               # React TypeScript application
├── backend/               # Node.js Express API
├── database/              # PostgreSQL database schemas and migrations
├── shared/                # Shared TypeScript types and utilities
├── infrastructure/        # Docker, deployment configs
└── docs/                  # Documentation
    ├── features/          # Feature specifications and plans
    ├── models/            # Data model documentation
    └── logs/              # Change and error logs
```

## Architecture Summary
- **Frontend**: React + TypeScript + Tailwind CSS (Dark theme)
- **Backend**: Node.js + Express + TypeScript
- **Database**: PostgreSQL with Prisma ORM
- **Authentication**: JWT with refresh tokens
- **Media Storage**: Azure Blob Storage / MinIO
- **Payments**: Stripe + Crypto (BlockChair API)
- **Monitoring**: Sentry for error tracking and security

## Key Features
1. Creator subscription platform
2. Multi-payment support (cards, crypto)
3. Role-based access control (Admin, Creator, User)
4. Media management with premium content gating
5. Support ticket system
6. Admin dashboard with full platform oversight

## Active Development
- Current Branch: main
- Last Updated: 2025-01-13
