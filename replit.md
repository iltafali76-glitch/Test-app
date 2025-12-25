# Replit Agent Configuration

## Overview

This is a full-stack warehouse order management application designed for sales teams to browse products, manage customers, create orders, and sync invoices to Xero. The application follows a monorepo structure with a React frontend (Vite), Express backend, and PostgreSQL database using Drizzle ORM.

## User Preferences

Preferred communication style: Simple, everyday language.

## System Architecture

### Frontend Architecture
- **Framework**: React 18 with TypeScript
- **Build Tool**: Vite with HMR support
- **Routing**: Wouter (lightweight React router)
- **State Management**: 
  - TanStack Query for server state
  - Zustand with persistence for cart state
- **UI Components**: shadcn/ui (Radix primitives + Tailwind CSS)
- **Styling**: Tailwind CSS with CSS variables for theming
- **Charts**: Recharts for dashboard analytics

### Backend Architecture
- **Framework**: Express.js with TypeScript
- **API Design**: REST endpoints defined in `shared/routes.ts` with Zod validation
- **Authentication**: Replit Auth integration using OpenID Connect with Passport.js
- **Session Storage**: PostgreSQL-backed sessions using connect-pg-simple

### Data Storage
- **Database**: PostgreSQL (required via DATABASE_URL environment variable)
- **ORM**: Drizzle ORM with drizzle-zod for schema validation
- **Schema Location**: `shared/schema.ts` contains all table definitions
- **Migrations**: Drizzle Kit with `db:push` command

### Key Data Models
- **Users**: Authentication with role-based access (admin/sales_person)
- **Customers**: Name, contact info, delivery address
- **Products**: Name, SKU, price, stock quantity, image URL
- **Orders**: Status tracking, Xero invoice sync, order items with quantities

### Authentication Flow
- Uses Replit's OIDC provider for authentication
- Session-based auth with 1-week TTL
- Protected routes via `isAuthenticated` middleware
- User data synced/upserted on login

### API Structure
Routes are defined declaratively in `shared/routes.ts` with:
- Method and path definitions
- Zod input validation schemas
- Response type schemas
- Used by both frontend (type-safe fetching) and backend (validation)

## External Dependencies

### Database
- PostgreSQL database required (DATABASE_URL environment variable)
- Session table must exist for Replit Auth

### Authentication
- Replit Auth (OIDC) - requires REPL_ID and SESSION_SECRET environment variables
- ISSUER_URL defaults to https://replit.com/oidc

### Third-Party Integrations
- **Xero**: Mocked sync endpoint at `/api/orders/:id/sync-xero` for invoice creation
- **Images**: Unsplash placeholders used for product images when not provided

### Key NPM Packages
- `drizzle-orm` / `drizzle-kit`: Database ORM and migrations
- `@tanstack/react-query`: Server state management
- `zustand`: Client state management (cart)
- `zod`: Runtime validation
- `passport` / `openid-client`: Authentication
- `recharts`: Dashboard charts
- Full shadcn/ui component library (Radix-based)