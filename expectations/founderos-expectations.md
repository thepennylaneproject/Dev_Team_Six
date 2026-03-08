# FounderOS — Expectations Document

> Source: `the_penny_lane_project/FounderOS/founder_report.md`
> Last reviewed: 2026-03-08

---

## 1. Language and Runtime Constraints

### 1.1 Next.js 14 App Router
FounderOS uses **Next.js 14 with the App Router**. Pages Router patterns are not permitted. File `warning` for any new route created outside the `src/app/` directory or using Pages Router conventions.

### 1.2 TypeScript
All source files must be TypeScript. Plain `.js` files in the application source (outside config files) must be filed `warning`.

### 1.3 Tailwind CSS
Styling uses Tailwind CSS. Introduction of another CSS framework or component library requires explicit approval. File `suggestion` for style solutions that bypass Tailwind.

---

## 2. API Architecture

### 2.1 All API routes in `src/app/api/**/route.ts`
Every API endpoint must be defined as a Next.js route handler at `src/app/api/**/route.ts`. Do not create API endpoints via a standalone Express/Fastify server or alternative routing pattern. File `critical` for any API endpoint outside this path convention.

### 2.2 Supabase middleware must protect `/api/*` routes
The Supabase middleware at `src/middleware.ts` must protect all `/api/*` routes **except** `/api/events/health`. Any new authenticated API route that bypasses the middleware must be filed `critical`. Any change that weakens or removes the health check exemption must be filed `warning`.

---

## 3. Database Constraints

### 3.1 PostgreSQL via `pg` pool for direct queries
Direct database queries use the `pg` connection pool defined in `src/lib/db.ts`. Do not bypass this pool for direct queries. File `warning` for any new direct DB query that imports `pg` outside of `src/lib/db.ts`.

### 3.2 Supabase client for user-scoped data
User-scoped data access must use the Supabase client (with user JWT). Do not use the service-role Supabase client for user-scoped queries. File `critical` for service-role client usage in user-data contexts.

### 3.3 No SQL injection surface
All SQL queries must use parameterized statements. Raw string interpolation into SQL is prohibited. File `critical` for any SQL injection surface found.

### 3.4 Multitenancy tables must be respected
The `organizations` and `organization_members` tables are the foundation of FounderOS's multitenancy model. All new routes that access user data must scope queries to the authenticated user's organization. File `critical` for any route that returns cross-tenant data.

---

## 4. Missing Dependency (Build-Breaking)

### 4.1 `@supabase/auth-helpers-nextjs` must be in `package.json`
`@supabase/auth-helpers-nextjs` is currently missing from `package.json`, which causes a build failure. This dependency must be present in `dependencies` before any deployment. File `critical` if this package is absent.

---

## 5. AI Routing

### 5.1 Multi-provider AI routing through `src/ai/AIRouter.ts`
All AI provider calls (OpenAI, Anthropic, etc.) must go through `src/ai/AIRouter.ts`. Direct calls to AI provider SDKs from route handlers or components are prohibited. File `critical` for any direct AI provider call that bypasses `AIRouter`.

### 5.2 AI API keys stored encrypted in `user_ai_settings`
All user-provided AI API keys must be stored encrypted in the `user_ai_settings` table. Plaintext storage of AI keys is prohibited. File `critical` for any plaintext API key storage.

---

## 6. Infrastructure

### 6.1 Docker multi-stage build must remain functional
The Docker multi-stage build must build and pass without errors. File `critical` if the Dockerfile is modified in a way that breaks the build.

### 6.2 Redis usage must be documented
Redis is declared in `docker-compose.yml`. Any active usage of Redis in the application (caching, queuing, pub/sub) must be documented in code comments or the relevant module's README. File `suggestion` for undocumented Redis usage.

---

## 7. Out-of-Scope Constraints

- Do not switch from Supabase without explicit approval
- Do not bypass Next.js middleware for authentication purposes
