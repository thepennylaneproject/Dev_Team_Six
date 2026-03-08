# ready — Expectations Document

> Source: `the_penny_lane_project/ready/ready_report.md`
> Last reviewed: 2026-03-08

---

## 1. Language and Runtime Constraints

### 1.1 TypeScript
All source files must be TypeScript. File `warning` for any `.js` source file added to `src/`.

### 1.2 React 19
The frontend uses React 19. File `warning` if React is downgraded below 19.

### 1.3 Vite build
The build system is Vite. File `warning` for any replacement of Vite.

### 1.4 Netlify Functions backend
All AI operations must go through Netlify Functions. File `critical` for any direct AI provider call made from frontend code.

---

## 2. Authentication and Database

### 2.1 Supabase Auth + RLS
Authentication uses Supabase Auth. All database tables must have RLS enabled. File `critical` for any new table without RLS policies.

---

## 3. AI Task Name Consistency

### 3.1 AI task names in `src/lib/ai/tasks.ts` must match caller invocations
The AI task names defined in `src/lib/ai/tasks.ts` must exactly match the names used in caller invocations throughout the codebase. Mismatches will cause silent failures at runtime. File `critical` for any mismatch between task name definitions and their call sites.

---

## 4. Missing Database Tables (Blocking Features)

### 4.1 Missing tables must be added to migrations before features are considered stable
The following tables are referenced in application code but do not yet exist in migrations:
- `learning_recommendations`
- `learning_paths`
- `rejection_analyses`
- `ai_invocations`
- `ai_cache`

File `critical` for any feature that is marked stable or shipped to users while its required table is absent from migrations.

---

## 5. Dependencies

### 5.1 `tailwindcss` must be declared and functional
`tailwindcss` configuration exists but the dependency may be missing from `package.json`. If the `tailwindcss` package is absent, the build will fail silently or produce unstyled output. File `critical` if `tailwindcss` is not declared in `package.json`.

---

## 6. User Tier Model

### 6.1 Tier names must be consistent: `free`, `pro`, `premium`, `coach`
The canonical user tier model uses four values: `free`, `pro`, `premium`, `coach`. References to `starter` or `admin` as user tiers must be reconciled with the canonical model. File `warning` for any code that references `starter` or `admin` as a user tier.

---

## 7. Code Quality

### 7.1 Lint must pass
`npm run lint` must pass without errors. The current lint configuration is failing and must be fixed. File `critical` for any linting configuration change that introduces or suppresses lint errors on `main`.

### 7.2 No hardcoded dashboard stats in production
Dashboard stat values (e.g., `78`, `12`, `4`, `5`) that are currently hardcoded must not appear in production builds. These must be replaced with real data fetched from the database. File `critical` for hardcoded numeric values in any component marked as production-ready.

---

## 8. Testing

### 8.1 Tests must be added before Alpha-to-Beta promotion
Any feature moving from Alpha to Beta status must have at least one automated test covering the critical path. File `warning` for any Alpha feature promoted to Beta without tests.

---

## 9. Out-of-Scope Constraints

- Do not expose AI operations directly to the client
- Do not deploy features dependent on missing database tables
- Do not allow tier-gated features to run with inconsistent tier names
