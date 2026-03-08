# Relevnt — Expectations Document

> Source: `the_penny_lane_project/Relevnt/relevnt_report.md`
> Last reviewed: 2026-03-08

---

## 1. Language and Runtime Constraints

### 1.1 TypeScript
All source files must be TypeScript. File `warning` for any `.js` source file added to `src/`.

### 1.2 React 19
The frontend uses React 19. File `warning` if React is downgraded below 19.

### 1.3 Vite build
The build system is Vite. File `warning` for any replacement of Vite with another bundler.

### 1.4 Netlify Functions backend
All privileged, AI, and admin operations must go through Netlify Functions. File `critical` for any privileged operation performed client-side.

---

## 2. Backend and Authentication

### 2.1 Netlify Functions require JWT + CSRF protection
All Netlify Functions that handle privileged or authenticated requests must validate the Supabase JWT **and** enforce CSRF protection. File `critical` for any function handling sensitive operations without both protections.

---

## 3. Database Constraints

### 3.1 Supabase PostgreSQL; 106 typed tables
The primary database is Supabase PostgreSQL with 106 typed tables. RLS is broadly enabled. Any new table added without RLS policies must be filed `critical`.

### 3.2 New migrations in `supabase/migrations/`
All new Supabase migrations must be placed in `supabase/migrations/`. Migrations placed in `migrations_archive/` or any other location will not be applied. File `critical` for any migration file placed outside `supabase/migrations/`.

### 3.3 `learning_search` edge function dependencies
The `learning_search` edge function requires the `learning_providers` and `learning_courses` tables to exist before the function goes live. File `critical` if the edge function is deployed or enabled without these tables present in migrations.

---

## 4. AI Integration

### 4.1 OpenAI SDK is the direct AI dependency
OpenAI is integrated via the official `openai` SDK. Anthropic, Gemini, DeepSeek, and AIMLAPI are integrated via custom HTTP fetch providers (not their official SDKs). Do not add official SDKs for non-OpenAI providers without explicit approval. File `warning` for any new AI provider SDK added without approval.

### 4.2 AI operations must go through Netlify Functions
All AI API calls must be made from Netlify Functions, not directly from the frontend. File `critical` for any direct AI API call in frontend code.

---

## 5. Data Compliance

### 5.1 `ts-jobspy` and aggregators must not violate source ToS
Data collected via `ts-jobspy` and job aggregator sources must not be stored in a way that violates the source platform's Terms of Service (e.g., bulk export, re-commercialization without permission). File `critical` for any data storage pattern that appears to violate ToS.

---

## 6. Billing

### 6.1 Stripe plan IDs must be real in production branches
Stripe configuration must use real, live plan IDs on production branches. Placeholder or test plan IDs in production configuration are prohibited. File `critical` for placeholder plan IDs on `main` or production branches.

---

## 7. Dependencies

### 7.1 `tailwindcss` must be declared in `package.json`
`tailwindcss` is referenced in the Tailwind config but may be missing from `package.json` `dependencies` or `devDependencies`. If absent, the build will fail. File `critical` if `tailwindcss` is not declared in `package.json`.

### 7.2 Husky + lint-staged must remain configured
Husky and lint-staged are configured for pre-commit hooks. These must remain active. Removal or disabling of pre-commit hooks requires explicit approval. File `warning` for any change that disables Husky or lint-staged.

---

## 8. Documentation

### 8.1 Root `README.md` must be created
No root `README.md` currently exists. A `README.md` must be created at the repo root before the project is considered stable. File `warning` if `README.md` is absent.

---

## 9. Out-of-Scope Constraints

- Do not replace Supabase with another auth/database provider without explicit approval
- Do not remove CSRF protection from any Netlify Function
- Do not expose raw job aggregator data to end users without processing
