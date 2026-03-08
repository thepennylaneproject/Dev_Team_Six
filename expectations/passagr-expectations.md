# Passagr — Expectations Document

> Source: `the_penny_lane_project/Passagr/passagr_report.md`
> Last reviewed: 2026-03-08

---

## 1. Language and Runtime Constraints

### 1.1 TypeScript
All source files must be TypeScript. Plain `.js` source files are not permitted. File `warning` for any new `.js` file added to `src/`.

### 1.2 React 19
The frontend uses React 19. Downgrading React requires explicit approval. File `warning` for a React version below 19.

### 1.3 Vite build
The frontend build system is Vite. Do not replace Vite with another bundler. File `warning` for build tooling changes.

### 1.4 Express API server
The backend API is an Express server. Do not introduce a second server framework alongside Express. File `warning` for any new server framework dependency.

---

## 2. Deployment Architecture

### 2.1 Frontend deployed to Netlify
The frontend (React/Vite) is deployed to Netlify. File `suggestion` for any configuration change that targets a different frontend hosting provider.

### 2.2 API deployed to `https://api.passagr.com`
The API is deployed separately at `https://api.passagr.com`. Hardcoding a different production API URL requires explicit approval. File `warning` for any environment configuration pointing to an unauthorized API URL.

---

## 3. Database Constraints

### 3.1 Supabase PostgreSQL with RLS enforced
The primary database is Supabase (managed PostgreSQL) with Row-Level Security enforced at the database layer. Any new table added without RLS policies must be filed `critical`.

### 3.2 User-scoped data through Supabase JWT + RLS
User-scoped data access must use the Supabase client authenticated with the user's JWT, allowing RLS to enforce data isolation. Do not use the service-role/admin client for user-scoped queries. File `critical` for service-role access to user data.

---

## 4. Authentication and Authorization

### 4.1 Admin endpoints require `x-admin-key` header
All admin API endpoints must validate the `x-admin-key` request header before processing. Admin endpoints without this check must be filed `critical`.

### 4.2 Cron/job endpoints require `x-cron-secret` header
All cron job and scheduled task endpoints must validate the `x-cron-secret` request header. File `critical` for any cron endpoint missing this validation.

---

## 5. AI Data Pipeline Constraints

### 5.1 AI-extracted data must land in `staging_country_research` first
All data extracted by the GPT-4 extractor (`workers/extractor.ts`) must be written to the `staging_country_research` table first. Direct writes to `countries` or `visa_paths` from the extractor are prohibited. File `critical` for any extractor path that bypasses staging.

### 5.2 Editorial review before publishing to production tables
The `editorial_reviews` table approval workflow must be completed before `publisher.ts` writes data to production tables. Publishing to production without an approved editorial review must be filed `critical`.

---

## 6. Content Integrity

### 6.1 User notes encrypted with AES-256-GCM
User notes must be encrypted using AES-256-GCM via the crypto utilities in `src/server/crypto/`. Plaintext storage of user notes is prohibited. File `critical` for any user note stored without encryption.

### 6.2 Typesense search index kept in sync via `search/sync_worker.ts`
The Typesense search index must be kept in sync through `search/sync_worker.ts`. Writes to production data tables that bypass the sync worker will result in stale search results. File `warning` for any data mutation that does not trigger a sync worker update.

### 6.3 Source URLs must pass `link_checker.ts` before being marked verified
Before a source URL is marked as verified, it must pass validation through `link_checker.ts`. File `warning` for any URL marked verified without passing link checker validation.

---

## 7. Privacy and Compliance

### 7.1 GDPR/privacy operations through Netlify Functions
GDPR data deletion, data export, and other privacy operations must be handled by the Netlify Functions in `netlify/functions/v1-privacy-*.ts`. Privacy operations implemented outside these functions must be filed `critical`.

---

## 8. Out-of-Scope Constraints

- Do not expose raw AI-extracted data to end users without editorial review
- Do not modify the AES-256-GCM encryption scheme without security review
- Do not bypass link validation when bulk-importing source data
