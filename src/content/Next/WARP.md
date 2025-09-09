# WARP.md

This file provides guidance to WARP (warp.dev) when working with code in this repository.

## Project Overview

This is a modern Next.js dashboard application built with TypeScript, React, and TailwindCSS. The project implements a responsive dashboard UI with multiple pages including dashboard, analytics, profile, and settings. The architecture is based on Next.js 15+ with the App Router pattern and follows modern React development practices.

## Technologies Used

### Core Framework & Runtime
- **Next.js 15.4.5+** - React framework with App Router
- **React 19.1.0** - UI library with latest concurrent features
- **TypeScript 5.9.2+** - Static type checking
- **Node.js** - JavaScript runtime environment

### Styling & UI
- **TailwindCSS 4.x** - Utility-first CSS framework with CSS variables
- **Shadcn/ui** - Primary component library (extends Radix UI primitives)
- **Radix UI** - Unstyled, accessible UI primitives
- **Lucide React** - Icon library
- **Class Variance Authority (CVA)** - Component variant management
- **Tailwind Merge** - Utility for merging Tailwind classes
- **Motion (Framer Motion)** - Animation library

### State Management & Data Fetching
- **TanStack Query (React Query) 5.85+** - Server state management and data fetching
- **Zustand** - Client-side state management (when needed)
- **Ky** - Modern HTTP client for API requests

### Authentication & Security
- **NextAuth.js 5.0 Beta** - Authentication library
- **IdentityServer4** - OAuth/OpenID Connect provider integration
- **JWT** - JSON Web Tokens for session management
- **Sentry** - Error monitoring and performance tracking

### Forms & Validation
- **React Hook Form 7.62+** - Form state management
- **Zod 4.x+** - Schema validation
- **@hookform/resolvers** - Integration between React Hook Form and Zod

### Development Tools
- **ESLint 9.x+** - Code linting with flat config
- **Prettier 3.6+** - Code formatting with Tailwind plugin
- **TypeScript** - Static type checking
- **PNPM** - Package manager
- **Cross-env** - Cross-platform environment variables

### Build & Deployment
- **Docker** - Containerization
- **Standalone Output** - Next.js standalone build for Docker
- **Bundle Analyzer** - Bundle size analysis
- **Webpack optimization** - Custom webpack optimizations

## Development Environment Setup

### Prerequisites Installation (Windows)

This project uses modern Node.js tooling. Follow these steps to set up your development environment:

#### 1. Install Node.js via FNM (Fast Node Manager)
```powershell
# Install fnm using winget
winget install Schniz.fnm

# Restart your terminal or reload your profile
# Add fnm to your PowerShell profile (run once)
fnm env --use-on-cd | Out-String | Invoke-Expression

# Install and use the latest LTS Node.js
fnm install --lts
fnm use lts-latest

# Verify installation
node --version
npm --version
```

#### 2. Enable Corepack and Setup PNPM
```powershell
# Enable corepack (comes with Node.js 16.10+)
corepack enable

# Prepare pnpm (will install the version specified in package.json)
corepack prepare pnpm@latest --activate

# Verify pnpm installation
pnpm --version

# If you need to update pnpm later
corepack prepare pnpm@latest --activate
```

#### 3. Alternative: Direct Tool Installation
If you prefer installing tools directly via winget:

```powershell
# Install Node.js directly (alternative to fnm)
winget install OpenJS.NodeJS

# Install pnpm directly (alternative to corepack)
winget install pnpm.pnpm
```

#### 4. Project Setup
```bash
# Clone the repository
git clone <repository-url>
cd praat

# Install dependencies
pnpm install

# Set up environment variables
cp .env.example .env.local
# Edit .env.local with your configuration

# Start development server
pnpm dev
```

### Additional Windows Development Tools
```powershell
# Install Git (if not already installed)
winget install Git.Git

# Install Windows Terminal (recommended)
winget install Microsoft.WindowsTerminal

# Install VS Code (recommended editor)
winget install Microsoft.VisualStudioCode

# Install Docker Desktop (for containerization)
winget install Docker.DockerDesktop
```

### Node.js Version Management
```bash
# This project uses Node.js 18+ (LTS recommended)
# Check current version
node --version

# If using fnm, switch versions easily
fnm list
fnm use 18
fnm use lts-latest

# Install specific version if needed
fnm install 20.10.0
fnm use 20.10.0
```

### Package Manager Notes
- This project uses **PNPM** as specified in `package.json` via `packageManager` field
- Corepack ensures everyone uses the same package manager version
- PNPM provides faster installs and better disk space efficiency
- Lock file: `pnpm-lock.yaml` (commit this to version control)

### Troubleshooting Setup Issues

#### FNM Issues
```powershell
# If fnm command not found after installation
# Add to PowerShell profile manually
notepad $PROFILE
# Add this line to the file:
# fnm env --use-on-cd | Out-String | Invoke-Expression

# Reload profile
. $PROFILE

# If .nvmrc support needed, create .node-version file instead
echo "18.19.0" > .node-version
```

#### Corepack Issues
```powershell
# If corepack enable fails
# Run PowerShell as Administrator
Start-Process powershell -Verb runAs
corepack enable

# If pnpm version mismatch errors
corepack prepare pnpm@10.15.0 --activate  # Use exact version from package.json

# Clear pnpm cache if needed
pnpm store prune
```

#### Permission Issues (Windows)
```powershell
# Set execution policy for scripts (run as Administrator)
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope LocalMachine

# If npm/pnpm global installs fail
npm config set prefix "%APPDATA%\npm"
# Add %APPDATA%\npm to your PATH environment variable
```

#### Network Issues
```bash
# If behind corporate firewall
npm config set registry https://registry.npmjs.org/
npm config set strict-ssl false  # Only if necessary

# For pnpm
pnpm config set registry https://registry.npmjs.org/

# Use company registry if available
pnpm config set registry https://your-company-registry.com/
```

## Common Commands

### Development
```bash
# Start development server with Turbopack
pnpm dev

# Start development server (standard)
next dev
```

### Building
```bash
# Standard production build
pnpm build

# Fast build without linting
pnpm build:fast

# Production build with environment
pnpm build:production

# Analyze bundle size
pnpm build:analyze
```

### Code Quality
```bash
# Run ESLint
pnpm lint

# Fix ESLint issues automatically
pnpm lint:fix

# Type checking
pnpm typescript:check

# Fast type checking (skip lib check)
pnpm typescript:check:fast
```

### Maintenance
```bash
# Clean build artifacts
pnpm clean

# Clean everything including cache
pnpm clean:all
```

### Docker
```bash
# Build Docker image
docker build -t praat .

# Run Docker container
docker run -p 3000:3000 praat
```

## Project Architecture

### Folder Structure
```
src/
├── app/                          # Next.js App Router pages
│   ├── (home)/                  # Route groups for layout organization
│   │   ├── dashboard/           # Dashboard page
│   │   ├── analytics/           # Analytics page  
│   │   ├── profile/             # Profile page
│   │   └── layout.tsx           # Private layout with sidebar
│   ├── auth/                    # Authentication pages
│   ├── api/                     # API routes
│   ├── layout.tsx               # Root layout
│   ├── globals.css              # Global styles
│   └── page.tsx                 # Root page (redirects)
├── components/                   # React components
│   ├── ui/                      # Shadcn/ui components
│   ├── layout/                  # Layout components (sidebar, topbar)
│   ├── providers/               # Context providers
│   └── [feature]/               # Feature-specific components
├── lib/                         # Utility functions and configurations
│   ├── utils.ts                 # General utilities (cn helper)
│   ├── auth.ts                  # NextAuth configuration
│   └── auth-helper.ts           # Auth utility functions
├── hooks/                       # Custom React hooks
├── queries/                     # TanStack Query configurations
├── constants/                   # Application constants
│   └── routes.ts                # Route definitions
├── actions/                     # Server actions
├── types/                       # TypeScript type definitions
└── styles/                      # Additional styles
```

### Key Architecture Patterns

#### 1. Next.js App Router with Route Groups
- Uses `(home)` route group for authenticated pages
- Separate layouts for public vs private routes
- File-based routing with TypeScript

#### 2. Authentication Flow
- **NextAuth.js** with IdentityServer4 provider
- JWT-based sessions with refresh token rotation
- Middleware for route protection
- Custom auth helpers for token validation
- Conditional auth wrapper for public/private routes

#### 3. Component Architecture
- **Shadcn/ui** as primary component library
- Composition over configuration
- Accessible components via Radix UI primitives
- Self-contained components (one per file)
- TypeScript interfaces for all props

#### 4. State Management Strategy
- **TanStack Query** for server state
- **Zustand** for complex client state (minimal usage)
- React state for local component state
- Query key factories for consistent cache management

#### 5. Styling Approach
- **Utility-first** with TailwindCSS
- **CSS Variables** for theming
- **Responsive design** mobile-first approach
- **Component variants** with CVA
- **Design system** through Shadcn/ui

## Development Standards & Patterns

### Component Standards
```typescript
// ✅ Good: Self-contained component with proper typing
"use client"; // Only when needed

import { cn } from "@/lib/utils";
import { Button } from "@/components/ui/button";

interface DashboardCardProps {
  title: string;
  value: string | number;
  description?: string;
  className?: string;
}

export function DashboardCard({ 
  title, 
  value, 
  description, 
  className 
}: DashboardCardProps) {
  return (
    <div className={cn("p-6 bg-card rounded-lg", className)}>
      <h3 className="text-sm font-medium text-muted-foreground">{title}</h3>
      <p className="text-2xl font-bold">{value}</p>
      {description && (
        <p className="text-xs text-muted-foreground">{description}</p>
      )}
    </div>
  );
}
```

### Form Standards (Shadcn + React Hook Form + Zod)
```typescript
"use client";

import { zodResolver } from "@hookform/resolvers/zod";
import { useForm } from "react-hook-form";
import { z } from "zod";
import { Button } from "@/components/ui/button";
import {
  Form,
  FormControl,
  FormDescription,
  FormField,
  FormItem,
  FormLabel,
  FormMessage,
} from "@/components/ui/form";
import { Input } from "@/components/ui/input";

const formSchema = z.object({
  username: z.string().min(2, "Username must be at least 2 characters"),
  email: z.string().email("Invalid email address"),
});

type FormData = z.infer<typeof formSchema>;

export function UserForm() {
  const form = useForm<FormData>({
    resolver: zodResolver(formSchema),
    defaultValues: {
      username: "",
      email: "",
    },
  });

  const onSubmit = (data: FormData) => {
    console.log(data);
  };

  return (
    <Form {...form}>
      <form onSubmit={form.handleSubmit(onSubmit)} className="space-y-6">
        <FormField
          control={form.control}
          name="username"
          render={({ field }) => (
            <FormItem>
              <FormLabel>Username</FormLabel>
              <FormControl>
                <Input placeholder="Enter username" {...field} />
              </FormControl>
              <FormDescription>
                This is your public display name.
              </FormDescription>
              <FormMessage />
            </FormItem>
          )}
        />
        <Button type="submit">Submit</Button>
      </form>
    </Form>
  );
}
```

### API Integration with TanStack Query + Ky
```typescript
// queries/users.ts
import { useQuery, useMutation, useQueryClient } from '@tanstack/react-query';
import ky from 'ky';

// Query key factory
export const userKeys = {
  all: ['users'] as const,
  lists: () => [...userKeys.all, 'list'] as const,
  list: (filters: UserFilters) => [...userKeys.lists(), { filters }] as const,
  details: () => [...userKeys.all, 'detail'] as const,
  detail: (id: string) => [...userKeys.details(), id] as const,
};

// API client with ky
const api = ky.create({
  prefixUrl: '/api',
  timeout: 10000,
  hooks: {
    beforeRequest: [
      request => {
        const token = getAuthToken();
        if (token) {
          request.headers.set('Authorization', `Bearer ${token}`);
        }
      }
    ]
  }
});

// Query hooks
export function useUsers(filters: UserFilters = {}) {
  return useQuery({
    queryKey: userKeys.list(filters),
    queryFn: () => api.get('users', { searchParams: filters }).json<User[]>(),
    staleTime: 5 * 60 * 1000, // 5 minutes
  });
}

export function useUser(id: string) {
  return useQuery({
    queryKey: userKeys.detail(id),
    queryFn: () => api.get(`users/${id}`).json<User>(),
    enabled: !!id,
  });
}

// Mutation hooks
export function useCreateUser() {
  const queryClient = useQueryClient();
  
  return useMutation({
    mutationFn: (userData: CreateUserData) => 
      api.post('users', { json: userData }).json<User>(),
    onSuccess: () => {
      queryClient.invalidateQueries({ queryKey: userKeys.lists() });
    },
  });
}
```

### Route Management
```typescript
// constants/routes.ts - All routes centralized
export const ROUTES = {
  home: "/" as const,
  dashboard: () => "/dashboard" as const,
  profile: () => "/user-profile" as const,
  
  // Nested routes
  jobs: {
    root: () => "/job" as const,
    detail: (id: string) => `/job/${id}` as const,
  } as const,
  
  api: {
    health: "/api/health",
    users: "/api/users",
  } as const,
} as const;

// Usage in components
import { ROUTES } from "@/constants/routes";
import Link from "next/link";

<Link href={ROUTES.dashboard()}>Dashboard</Link>
<Link href={ROUTES.jobs.detail("123")}>Job Details</Link>
```

### Authentication Patterns
```typescript
// Server component auth check
import { auth } from "@/lib/auth";
import { redirect } from "next/navigation";
import { ROUTES } from "@/constants/routes";

export default async function ProtectedPage() {
  const session = await auth();
  
  if (!session) {
    redirect(ROUTES.auth.signIn());
  }
  
  return <div>Protected content</div>;
}

// Client component auth check
"use client";
import { useSession } from "next-auth/react";

export function UserProfile() {
  const { data: session, status } = useSession();
  
  if (status === "loading") return <div>Loading...</div>;
  if (status === "unauthenticated") return <div>Access denied</div>;
  
  return <div>Welcome {session?.user?.name}</div>;
}
```

### Environment Configuration
```typescript
// Environment variables pattern
AUTH_IDENTITY_SERVER4_ID=your_client_id
AUTH_IDENTITY_SERVER4_ISSUER=https://your-identity-server.com
AUTH_BASE_URL=https://your-app.com
NEXTAUTH_SECRET=your-secret-key
NEXTAUTH_URL=https://your-app.com
SENTRY_DSN=your-sentry-dsn
```

## Component Library Standards

### Shadcn/ui Usage
- **Primary choice** for all UI components
- Prefer Shadcn components over native HTML elements
- Extend via composition, never modify source files
- Add new components: `pnpm dlx shadcn@latest add [component]`

### Available Components Catalog
Key Shadcn components in use:
- Forms: `form`, `input`, `textarea`, `select`, `checkbox`, `radio-group`
- Navigation: `navigation-menu`, `breadcrumb`, `pagination`
- Feedback: `toast`, `alert-dialog`, `progress`, `spinner`
- Layout: `card`, `separator`, `tabs`, `accordion`
- Data: `table`, `data-table`, `calendar`, `date-picker`

## Performance Optimizations

### Next.js Optimizations
- **Standalone output** for Docker deployment
- **Package import optimization** for common libraries
- **Webpack build worker** enabled
- **Tree shaking** and deterministic module IDs
- **Bundle analyzer** integration

### React Query Optimizations
- **Query key factories** for consistent cache management
- **Stale time** configuration for reduced refetching
- **Background refetching** for better UX
- **Optimistic updates** for mutations

### Build Optimizations
- **Turbopack** for faster development builds
- **TypeScript incremental builds** 
- **ESLint caching** disabled for accurate results
- **Webpack compression** for production builds

## Testing Strategy

### Tools & Approach
- Focus on **integration testing** over unit testing
- **TypeScript** for compile-time error catching
- **ESLint rules** for code quality
- **Manual testing** for UI/UX validation

### Key Testing Areas
- Authentication flows and token refresh
- Form validation and error handling
- API integration and error states
- Responsive design across devices
- Navigation and route protection

## Deployment & Infrastructure

### Docker Configuration
- **Multi-stage build** for optimization
- **Standalone Next.js output** for minimal image size
- **Environment variable** injection at runtime
- **Health checks** for container monitoring

### Production Considerations
- **Sentry integration** for error monitoring
- **Performance tracking** with Core Web Vitals
- **Bundle size monitoring** with analyzer
- **Security headers** via middleware

## Code Quality Standards

### TypeScript
- **Strict mode** enabled
- **Type safety** prioritized over `any`
- **Interface definitions** for all props
- **Consistent import styles** with inline type imports

### ESLint Configuration
- **Flat config** format (ESLint 9+)
- **TypeScript rules** for type safety
- **React hooks** exhaustive deps disabled (controlled manually)
- **Prettier integration** for formatting

### File Organization
- **One component per file** for maintainability
- **Feature-based organization** for components
- **Consistent naming** conventions (kebab-case for files)
- **Clear separation** between server and client components

## Security Best Practices

### Authentication & Authorization
- **JWT tokens** with automatic refresh
- **CSRF protection** via NextAuth.js
- **Secure cookie** configuration
- **Route-based protection** with middleware

### API Security
- **Bearer token** authentication
- **Request timeout** configuration
- **Error boundary** implementation
- **Input validation** with Zod schemas

This documentation provides the foundation for consistent development practices across the project. When working on new features, follow these established patterns and technologies to maintain code quality and consistency.
