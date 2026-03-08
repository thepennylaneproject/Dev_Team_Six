SECTION 1: PROJECT IDENTITY
Project name

package.json name: ready
JSON
/home/runner/work/ready/ready/package.json
{
"name": "ready"
}
README title: “Ready — Career Interview Preparation Platform”
md
/home/runner/work/ready/ready/README.md

# Ready — Career Interview Preparation Platform

Repository URL

[Verified] Git remote:
Text
origin https://github.com/thepennylaneproject/ready
One-line description

Exact README description:
“A React + TypeScript + Vite app deployed on Netlify. Helps job seekers prepare for interviews with AI-powered coaching, skill-gap analysis, LinkedIn/portfolio review, and readiness scoring.”

Cleaner version:
[Inference from README] An AI-assisted interview preparation app for job seekers, combining profile analysis, mock interviews, skill-gap analysis, coaching, and readiness scoring.
Project status

Alpha
Rationale [Inference from code + validation]:
Auth, onboarding, routes, Netlify deployment, Supabase schema, and multiple feature surfaces are present.
npm run ci passes, so it is buildable.
But npm run lint fails heavily, there are no tests, several frontend features reference DB tables/columns not present in migrations, and some AI task names do not line up with the declared task registry.
That is beyond “prototype scaffolding,” but not solid enough for Beta.
First commit date and most recent commit date

On origin/main:
First commit: 2025-12-31 16:47:17 -0600
Most recent commit: 2026-03-06 19:25:59 -0600
[Verified via git log origin/main]
Note: current working audit branch has one newer planning commit from 2026-03-07, but that is not on main.
Total number of commits

origin/main: 30 commits
Current audit branch: 31 commits
[Verified]
Deployment status

[Verified] README says the app is deployed to Netlify.
[Verified] netlify.toml exists with build, publish, and functions config.
[Verified] GitHub Actions CI exists.
[Not found in codebase] No committed production site hostname for Ready itself.
Live URL(s) if discoverable in config

[Not found in codebase] No explicit public Ready site URL.
[Verified] Relative Netlify health endpoint:
Text
/.netlify/functions/health
[Verified external sister-product URL]
Text
https://relevnt.work
SECTION 2: TECHNICAL ARCHITECTURE
Primary language(s) and frameworks

Layer Tech Version / Source
Frontend language TypeScript ~5.9.3
Frontend framework React ^19.2.0
Routing React Router DOM ^7.11.0
Build tool Vite ^7.2.4
Styling Tailwind CSS ^4.1.18
Backend runtime Netlify Functions @netlify/functions ^5.1.2
Auth / DB client Supabase JS ^2.89.0
AI SDK OpenAI ^6.15.0
Icons lucide-react ^0.562.0
Full dependency list

Core framework dependencies
react
react-dom
react-router-dom
UI/styling libraries
tailwindcss
@tailwindcss/vite
autoprefixer
postcss
lucide-react
canvas-confetti
@types/canvas-confetti
State management
[Verified] No Redux/Zustand/MobX/etc.
Uses:
React Context: AuthContext, ReadyThemeProvider, ToastProvider
local component state/hooks
API/data layer
@supabase/supabase-js
@netlify/functions
AI/ML integrations
Direct dependency:
openai
Provider code present via fetch:
Anthropic
AIML API
Google Gemini
DeepSeek
Authentication/authorization
@supabase/supabase-js
Testing
[Not found in codebase] No Jest/Vitest/Cypress/Playwright test dependency for app testing.
Build tooling
vite
typescript
@vitejs/plugin-react
eslint
@eslint/js
typescript-eslint
eslint-plugin-react-hooks
eslint-plugin-react-refresh
globals
@types/node
@types/react
@types/react-dom
Other notable dependencies
None beyond above.
Project structure

Text
/home/runner/work/ready/ready
├── .github/
│ └── workflows/ # CI workflow(s)
├── audits/
│ ├── findings/ # Prior audit outputs
│ └── runs/ # Audit run artifacts
├── docs/
│ └── design-system/ # Design-system notes and investor doc
├── netlify/
│ └── functions/ # Serverless backend/API functions
├── public/
│ ├── brand/ # Logos/favicons/marks
│ ├── icons/ # Custom SVG icon assets
│ └── textures/ # Texture assets
├── src/
│ ├── assets/ # App assets
│ ├── components/ # Reusable and feature UI components
│ ├── config/ # Cross-product + icon config
│ ├── contexts/ # Auth and theme context
│ ├── hooks/ # Data/AI/session hooks
│ ├── lib/ # Supabase + readiness + AI task defs
│ ├── pages/ # Route pages
│ ├── services/ # Client-side AI service wrapper
│ ├── shared/ # Shared TS types
│ ├── styles/ # Theme tokens and feature CSS
│ └── types/ # Domain TS interfaces
├── supabase/
│ └── migrations/ # SQL schema and RLS migrations
├── README.md # Setup + product overview
├── netlify.toml # Netlify deployment config
├── package.json # Scripts and dependencies
└── vite.config.ts # Vite config
Architecture pattern

[Inference from code] SPA + serverless backend + direct Supabase client access
More specifically:
React/Vite single-page app
Netlify Functions for AI and protected server-side operations
Browser talks directly to Supabase for many CRUD reads/writes
Netlify Functions use Supabase admin or authenticated clients
Data flow

User loads SPA.
AuthContext gets Supabase session and user profile.
UI pages either:
query Supabase directly from browser using anon key + RLS, or
call /.netlify/functions/... with a **\*\*** for AI/protected work.
Function verifies JWT, gets user tier/profile, optionally calls external AI/API provider, persists results to Supabase, returns JSON.
UI stores/display results and sometimes persists additional data client-side.
Database/storage layer

Verified committed schema in migrations
Table Columns
ready_profiles id, display_name, headline, goal, focus_areas, readiness_score, last_practice_date, assessments_completed, created_at, updated_at, later migration adds tier
interview_prep id, user_id, position, company, questions, ai_feedback, created_at, updated_at
interview_practice_sessions id, user_id, interview_prep_id, questions, practice_data, status, overall_feedback, created_at, updated_at
linkedin_profiles id, user_id, linkedin_url, profile_data, analysis_results, is_public, share_token, created_at, updated_at
portfolio_analyses id, user_id, portfolio_url, analysis_results, is_public, share_token, created_at, updated_at
skill_gap_analyses id, user_id, target_role, current_skills, gaps, strengths, action_plan, status, created_at, updated_at
learning_progress id, user_id, skill_gap_id, skill, resource_url, resource_name, status, completed_at, created_at
career_narratives id, user_id, origin_story, pivot_explanation, value_proposition, future_vision, voice_settings, created_at, updated_at
negotiation_sessions id, user_id, position, company, offer_details, target_range, strategy, scripts, created_at, updated_at
readiness_snapshots id, user_id, overall_score, practice_score, assessment_score, skills_score, narrative_score, snapshot_date, created_at
interview_sessions id, user_id, interview_prep_id, question, user_answer, feedback, score, created_at
Functions / triggers
handle_updated_at()
handle_new_ready_user() trigger on auth.users
Referenced in app code but not found in committed migrations
[Gap]
Tables:
learning_recommendations
learning_paths
rejection_analyses
ai_invocations
ai_usage
ai_cache
profiles (used by netlify/functions/ai.ts)
Columns referenced but not in committed schema:
skill_gap_analyses.missing_skills
skill_gap_analyses.recommendations
skill_gap_analyses.skill_gaps
career_narratives.profile_snapshot
career_narratives.narratives
negotiation_sessions.job_title
negotiation_sessions.offer_salary
negotiation_sessions.target_min
negotiation_sessions.target_max
negotiation_sessions.responses
API layer

Route/path Method Purpose Auth required
/.netlify/functions/health No method guard Health check No
/.netlify/functions/ai POST Generic AI endpoint Yes
/.netlify/functions/analyze_rejection POST Rejection coaching analysis Yes
/.netlify/functions/interview_prepare POST Generate interview prep/questions Yes
/.netlify/functions/interview_evaluate POST Score interview answer Yes
/.netlify/functions/linkedin_profile_analyze POST Fetch + analyze LinkedIn profile Yes
/.netlify/functions/portfolio_analyze POST Analyze portfolio URL/content Yes
/.netlify/functions/skill_gap_analyze POST AI skill-gap analysis Yes
/.netlify/functions/learning_recommendations POST Generate curated learning recommendations Yes
/.netlify/functions/career_narrative POST Generate career narrative Yes
/.netlify/functions/readiness_score GET Compute readiness score and milestone Yes
/.netlify/functions/get_practice_performance No method guard Aggregate interview session stats Yes
External service integrations

Supabase
Auth, DB, RLS-backed data access
Netlify
Deployment + serverless functions
OpenAI
AI generation via official SDK
Anthropic
Claude API via fetch
AIML API
Chat completion provider
Google Gemini
Provider code exists
[Gap] Not wired into active model registry in run.ts
DeepSeek
Provider code exists
[Gap] Not wired into active model registry in run.ts
RapidAPI LinkedIn Data API
Used to fetch LinkedIn profile data
Cloudinary
Theme hero/texture images
Relevnt
Cross-product outbound links only, not an API integration in this repo
AI/ML components

Providers/models in active registry
AIML API: gpt-4o-mini, claude-3-haiku
OpenAI: gpt-4o-mini, gpt-4o
Anthropic: claude-3-5-sonnet-20241022
Prompting
buildSystemPrompt() constructs a generic “Ready AI coaching assistant” system prompt.
JSON tasks get schema instructions and “JSON only” formatting constraints.
Legacy router builds task-specific instructions plus payload dump.
Processing
Inputs serialized to JSON/text
Outputs normalized via JSON normalizer for structured tasks
Results may be cached and logged to telemetry tables
Returned to UI and often persisted to Supabase
Important gap
[Verified] Task name mismatches exist:
src/lib/ai/tasks.ts defines tasks like interview-prep, salary-negotiation, portfolio-analyze
but some callers use practice_question_gen, legacy_structured, legacy_text
This suggests some AI flows are code-present but runtime reliability is questionable.
Authentication and authorization model

Login/signup: email + password via Supabase Auth
Profile creation: trigger on auth.users inserts into ready_profiles
Onboarding gate: users without goal in profile are forced through onboarding
Authorization:
Protected frontend routes via ProtectedRoute
API auth via **\*\*** verified server-side
DB access via Supabase RLS
Permission levels
free
pro
premium
coach
[Gap] Client AI typing also references starter and admin, which do not match the core user tier model.
Environment variables

Supabase / auth
VITE_SUPABASE_URL
VITE_SUPABASE_ANON_KEY
SUPABASE_URL
SUPABASE_ANON_KEY
SUPABASE_SERVICE_ROLE_KEY
AI providers
OPENAI_API_KEY
ANTHROPIC_API_KEY
AIMLAPI_API_KEY
DEEPSEEK_API_KEY
GOOGLE_API_KEY
Third-party enrichment
RAPIDAPI_API_KEY
AI limits / runtime
AI_MAX_ATTEMPTS
[Pattern used in code]
AI_CAP_FREE_DAILY
AI_CAP_PRO_DAILY
AI_CAP_PREMIUM_DAILY
AI_CAP_COACH_DAILY
AI_CAP_FREE_HIGH_DAILY
AI_CAP_PRO_HIGH_DAILY
AI_CAP_PREMIUM_HIGH_DAILY
AI_CAP_COACH_HIGH_DAILY
CORS / deployment platform
ALLOWED_ORIGIN
DEPLOY_PRIME_URL
URL
Node/runtime
NODE_ENV
SECTION 3: FEATURE INVENTORY
Feature name User-facing description Completeness Key files Dependencies
Auth (login/signup/logout) Create account and sign in with email/password Functional src/pages/Login.tsx, src/pages/Signup.tsx, src/contexts/AuthContext.tsx, src/lib/supabase.ts Supabase Auth
Onboarding Set goal and focus areas before using app Functional src/pages/Onboarding.tsx, src/components/onboarding/OnboardingGate.tsx, supabase/migrations/20250601_add_onboarding_cols.sql Auth, ready_profiles
Dashboard Overview page with readiness, recommendations, quick stats Partial src/pages/Dashboard.tsx, src/components/Dashboard/ReadinessScore.tsx, src/components/Dashboard/QuickStats.tsx, src/components/Dashboard/RecommendedAction.tsx Auth/profile
Mirror (LinkedIn + portfolio analysis) Analyze LinkedIn and portfolio, score readiness, toggle sharing Partial src/pages/Mirror.tsx, netlify/functions/linkedin_profile_analyze.ts, netlify/functions/portfolio_analyze.ts, supabase/migrations/001_ready_schema.sql Auth, AI, RapidAPI, Supabase
Public mirror sharing Make mirror results public and copy share URL Scaffolded / Partial src/pages/Mirror.tsx Mirror data
Practice Center Generate interview prep templates and start sessions Partial src/pages/PracticeCenter.tsx, netlify/functions/interview_prepare.ts, src/pages/PracticeSession.tsx, src/hooks/usePracticeSession.ts Auth, AI, interview_prep, interview_practice_sessions
Interview answer evaluation Score answers and save session/question history Partial netlify/functions/interview_evaluate.ts, src/pages/PracticeSession.tsx, supabase/migrations/20260101_add_interview_sessions.sql Practice, AI
Skills Gap Analyzer Compare experience to target role and track addressed gaps Partial src/pages/SkillsGap.tsx, src/hooks/useAnalyzeSkillsGap.ts, netlify/functions/skill_gap_analyze.ts Auth, AI, skill_gap_analyses
Learning Paths Recommend resources and track completion Partial src/pages/LearningPaths.tsx, netlify/functions/learning_recommendations.ts, src/hooks/useLearningPaths.ts Skills gap, missing DB tables
Rejection Debrief Paste rejection email and get next-step coaching Partial src/components/coaching/RejectionCoaching.tsx, netlify/functions/analyze_rejection.ts Auth, AI, missing rejection_analyses table
Negotiation Coach Generate negotiation strategy and scripts Partial src/components/coaching/NegotiationCoach.tsx, src/hooks/useAITask.ts, netlify/functions/ai.ts Auth, AI, negotiation_sessions
Career narrative generation Generate interview/career narrative Partial netlify/functions/career_narrative.ts, src/lib/readiness.ts, supabase/migrations/001_ready_schema.sql Auth, AI, readiness
Playback / progress tracking See readiness progress, practice trends, profile scores Partial src/pages/Playback.tsx, src/hooks/usePerformance.ts, netlify/functions/readiness_score.ts Data from practice/assessment/skills
“You’re Ready” milestone Celebration page with confetti and next CTAs Functional / data-dependent src/pages/YoureReady.tsx, src/lib/readiness.ts, netlify/functions/readiness_score.ts Readiness snapshots
Settings Manage account preferences/integrations Scaffolded src/pages/Settings.tsx None substantial
Cross-product handoff to Relevnt Open jobs/applications in sister app Functional src/config/cross-product.ts, src/pages/Playback.tsx, src/pages/YoureReady.tsx Relevnt URL config
Notable feature-level evidence of incompleteness

Dashboard stats/readiness are partly hardcoded (78, 12, 4, 5).
Some features write/read schema elements not present in migrations.
Sharing UI creates /shared/mirror/... URLs, but no matching route exists in src/App.tsx.
SECTION 4: DESIGN SYSTEM & BRAND
Color palette

[Verified] No tailwind.config.\* file found.
Primary color sources:
src/styles/tokens.css
src/contexts/ReadyThemeProvider.tsx
docs/design-system/pro-parity-map.md
Name Hex Defined in
--bg-1 #E8EBE8 src/styles/tokens.css
--bg-2 #D9DED9 src/styles/tokens.css
--panel-1 #E8EBE8 src/styles/tokens.css
--panel-2 #D9DED9 src/styles/tokens.css
--accent-1 #4E808D src/styles/tokens.css
--accent-2 #81B5B6 src/styles/tokens.css
--accent-3 #A8C3A2 src/styles/tokens.css
--tier-accent #6E9E8C src/styles/tokens.css
--ready-state #A8C3A2 src/styles/tokens.css
--color-text #1A2024 src/styles/tokens.css
--color-text-secondary #4A5560 src/styles/tokens.css
--color-text-muted #6B7280 src/styles/tokens.css
--color-primary-hover #3D6A75 src/styles/tokens.css
--color-secondary-hover #8FB08A src/styles/tokens.css
--color-success #22C55E src/styles/tokens.css
--color-warning #F59E0B src/styles/tokens.css
--color-error #EF4444 src/styles/tokens.css
Dark --bg-1 #0C0F12 src/styles/tokens.css
Dark --bg-2 #1A2024 src/styles/tokens.css
Dark --panel-2 #242A2F src/styles/tokens.css
Dark text #F7F8F7 src/styles/tokens.css
Dark secondary text #B8C0C8 src/styles/tokens.css
Dark muted text #8A9299 src/styles/tokens.css
Dark primary hover #6A9AA6 src/styles/tokens.css
Dark secondary hover #B8D4B3 src/styles/tokens.css
Theme provider colors

ReadyThemeProvider duplicates many of the above and also defines:
Light surface: #FDFCFA
Light surfaceHover: #F5F3F0
Dark success: #7AA28F
Dark warning: #D4A574
Dark error: #C18686
Important inconsistency

[Verified] tokens.css and ReadyThemeProvider do not fully agree on surface values.
Typography

Fonts defined:
Inter
Roboto
Scale:
--text-hero: 72px
--text-h1: 48px
--text-h2: 32px
--text-h3: 24px
--text-body: 18px
--text-body-sm: 16px
--text-caption: 14px
--text-small: 12px
[Gap] var(--font-serif) is used in Mirror CSS, but no --font-serif token was found.
Component library

[Verified] Shared component system exists.
Reusable components found:
Component Description
Button Button with variants/sizes/loading/full-width
Input Styled form input with labels/errors/helper text
Card Reusable container card
Alert Alert messaging
ToastProvider / useToast In-app toast notifications
LoadingState Skeleton/loading components
ConfirmDialog Confirmation modal/dialog
ErrorBoundary App-level React error boundary
EmptyState Empty-state UI
Icon Custom SVG icon component
Container Max-width layout wrapper
Spinner Loading spinner
Modal Generic modal
ReadinessGauge Circular readiness gauge
PageBackground Decorative background wrapper
AppLayout Header/footer shell
Header Main navigation/header
Footer Footer
PageShell Layout/page wrapper
StatCard Small metric card
FeatureCard Feature tile card
Design language

[Inference from tokens, comments, and components]
Calm, premium, editorial career-coaching aesthetic:
muted sage/teal palette
soft cards and rounded corners
“Pro tier” visual language
gentle, supportive coaching copy
This feels more “premium coaching SaaS” than “startup dashboard neon.”
Responsive strategy

Mix of:
Tailwind responsive utilities (sm:, md:, lg:)
custom CSS media queries in feature CSS (max-width: 768px, 850px, 900px, etc.)
reusable size-based layout primitives (Container, Modal, ReadinessGauge)
[Verified] Mobile nav drawer exists in header.
Dark mode

[Verified] Supported.
Implemented by:
ReadyThemeProvider
data-theme="dark" / .dark CSS token overrides
persisted in localStorage key ready-theme-mode
optional non-React initializer in public/theme.js
Brand assets

Logos / marks in public/brand/:
logo-dark.svg
logo-light.svg
primary-horizontal-dark.svg
primary-horizontal-light.svg
stacked-dark.svg
stacked-light.svg
mark.svg
mark-only-fullcolor.svg
mark-only-fullcolor-512.svg
mark-only-mono-black.svg
mark-only-mono-white.svg
favicons / touch icons
Custom icons in public/icons/light and public/icons/dark
Texture assets:
public/textures/background-texture-dark.png
public/textures/background-texture-light.png
Critical asset issue
[Verified] Both committed texture PNGs contain Cloudinary 404 HTML error pages, not valid image binary content.
SECTION 5: DATA & SCALE SIGNALS
User model

Core per-user profile data in ready_profiles:
display name, headline, goal, focus areas, readiness score, practice date, assessments completed, tier, timestamps
Related per-user data:
interview prep
practice sessions
interview sessions
LinkedIn analyses
portfolio analyses
skill-gap analyses
learning progress
career narratives
negotiation sessions
readiness snapshots
User journey from signup to value

Sign up / log in
Profile auto-created via DB trigger
Onboarding collects goal + focus areas
User enters feature flows (Mirror, Practice, Skills, Coaching)
Progress feeds readiness score
If threshold crossed, “You’re Ready” page pushes them to Relevnt
Content/data volume

[Not found in codebase] No seed files, fixtures, or sample datasets for product data.
Signals of expected operating scale:
many queries use low limits: 1, 5, 20, 50, 200
product seems designed around relatively small per-user histories, not large multitenant analytics workloads
Performance considerations

Positive signals:
explicit column projection in several pages
Promise.all used in some data fetching
AI cache abstraction (memoryCache + ai_cache)
provider circuit breaker in AI telemetry
capped queries with .limit(...)
Negative signals:
main JS bundle is large: ~533 kB minified
Vite warns about chunk size
no route-level code splitting found
pagination helper exists, but little product use of true pagination
cache tables (ai_cache, ai_invocations, ai_usage) are referenced but not in migrations
Analytics/tracking

[Not found in codebase] No product analytics SDK like PostHog, Segment, Plausible, Amplitude, Mixpanel, GA.
[Verified] AI telemetry exists in backend:
invocation logging
provider success/failure tracking
cost estimation
This is operational telemetry, not user analytics.
Error handling

Frontend:
ErrorBoundary wraps app root
toasts show user-facing failures
frequent console.error
Backend:
JWT verification helpers
generic JSON error responses
CORS helper + response helper
[Gap] Two response helper systems exist:
utils/supabase.ts uses restricted origin CORS
utils/response.ts still defines wildcard \*
utils/response.ts appears unused, but it is stale/dead security-sensitive code.
Testing

[Verified] No test files found matching \*_/_.{test,spec}.{ts,tsx,js,jsx}
[Verified] No testing framework dependency found in package.json
What does exist:
build/typecheck/lint scripts
audit artifacts in /audits (not executable tests)
SECTION 6: MONETIZATION & BUSINESS LOGIC
Pricing/tier structure

[Verified in code]
netlify/functions/ai/utils/cost-tracking.ts defines:
free → $0/month
pro → $19/month
premium → $49/month
coach → $99/month
Payment integration

[Not found in codebase] No Stripe, PayPal, Paddle, billing portal, webhook, checkout, or invoicing integration.
Subscription/billing logic

[Partial / code-only]
Tier pricing and feature descriptions exist in code.
Daily AI caps exist in telemetry.
Client task limits exist in TS types.
[Not found in codebase] No recurring billing workflow, no subscription state machine, no trial logic.
Feature gates

Tier logic appears in:
utils/auth.ts -> getUserFeatures()
ai/run.ts -> TIER_MAX_QUALITY
ai/telemetry.ts -> daily/high-quality caps
src/types/ai-responses.types.ts -> TIER_LIMITS
Examples:
free: limited practice/assessment counts, basic AI
pro: more AI + skill-gap
premium/coach: broader AI access, unlimited-like caps
[Gap] Multiple tier systems are inconsistent:
free/pro/premium/coach
starter/free/pro/premium/admin
Usage limits

Daily AI caps in telemetry:
free: 25
pro: 200
premium: 1000
coach: 1200
High-quality daily caps:
free: 0
pro: 10
premium: 1000
coach: 1200
Client task limits also exist per task.
[Gap] Backend billing enforcement and UI plan management are missing.
SECTION 7: CODE QUALITY & MATURITY SIGNALS
Code organization

Good high-level separation:
pages
reusable components
hooks
contexts
serverless functions
SQL migrations
But there is clear drift between layers:
frontend types
backend writes
migrations
Patterns and conventions

Patterns used:
React Context for auth/theme/toasts
hooks for data/AI/session flows
serverless function “BFF” layer
shared Supabase auth/response helpers
Naming consistency:
mixed
Examples of drift:
practice_prep_id vs interview_prep_id
position vs job_title
scripts vs responses
gaps vs missing_skills/skill_gaps
Comments still say RELEVNT in several Ready files, suggesting code adaptation from a sister project.
Documentation

README quality: good
setup
env vars
scripts
deployment
tech stack
Additional docs:
investor description
design-system parity note
audit artifacts
Inline comments/docblocks: common
Architecture docs: [Partial] implied, not formalized
TypeScript usage

TypeScript is enabled and strict build passes.
However lint shows many issues:
widespread any
unused vars
purity/React-refresh violations
So TS adoption is meaningful but not clean.
Error handling patterns

Fairly consistent generic 500s server-side
user-facing error toasts client-side
error boundary at root
[Gap] still mostly console.error + generic handling; no centralized observability service
Git hygiene

Signals:
PR merge commits into main
many copilot/... feature/audit branches
many “Initial plan” commits
Summary:
structured PR flow exists
commit-message quality is mixed
automation/bot activity is significant
Technical debt flags

TODO found:
readiness milestone email notification not implemented
Additional debt flags [Verified]:
schema drift
dead/unused helper (utils/response.ts)
provider code present but not wired
broken texture assets
missing tests
failing lint
unsupported share route
hardcoded dashboard/header stats
Security posture

Positive:
Supabase Auth
RLS enabled in migrations
token verification in functions
parameterized Supabase queries
restricted-origin CORS helper in utils/supabase.ts
.env patterns in .gitignore
Negative / current issues:
npm audit currently reports 5 vulnerabilities (2 moderate, 3 high), including:
react-router / react-router-dom
rollup
minimatch
ajv
stale wildcard-CORS response helper still exists in repo
Prior audit evidence:
audits/run_security-audit-01.json documents several fixed security issues
Current validation state

npm run ci ✅
npm run lint ❌
npm audit ❌ 5 vulnerabilities
SECTION 8: ECOSYSTEM CONNECTIONS
Shared code or patterns with other Penny Lane projects

[Verified]
src/types/index.ts begins with RELEVNT -> READY TYPE DEFINITIONS
Icon.tsx and Input.tsx comments still describe Relevnt
design-system doc says Ready uses Relevnt's Pro tier
This strongly suggests code/design extraction or adaptation from Relevnt.
Shared dependencies or infrastructure

[Verified] Shared visual language with Relevnt is documented.
[Not found in codebase] Same Supabase instance or same Netlify account cannot be proven from checked-in files.
Data connections

[Not found in codebase] No direct shared database connection to another Penny Lane product is explicitly configured.
[Inference from comments only] Migration comments and reused types suggest shared schema ancestry, not necessarily shared runtime data.
Cross-references

Explicit to Relevnt only:
src/config/cross-product.ts
docs/investor-description.md
CTAs in Playback and YoureReady
[Not found in codebase] No references to Codra, Mythos, embr, passagr, or advocera.
SECTION 9: WHAT'S MISSING (CRITICAL)
Gaps for a production-ready product

Schema reconciliation
Frontend/backend reference tables and columns not present in committed migrations.
AI runtime correctness
Task-name mismatches (legacy_structured, practice_question_gen, etc.) threaten core AI flows.
Automated testing
No unit/integration/e2e coverage.
Lint/code health
Lint is failing broadly.
Billing and entitlement plumbing
Pricing exists; payments do not.
Gaps for investor readiness

[Not found in codebase]
No product analytics instrumentation
No user growth/usage metrics
No documented uptime/SLOs
No formal architecture diagram
No release/versioning strategy
No evidence of real customer traffic or telemetry dashboards
No committed ready-site production URL
Gaps in the codebase itself

Missing DB migrations for referenced entities:
learning_recommendations
learning_paths
rejection_analyses
ai_invocations
ai_usage
ai_cache
profiles
Type/schema drift across frontend/backend/SQL
utils/response.ts appears unused
Google/DeepSeek provider code present but not active
texture assets are broken
share URLs point to non-existent routes
CI workflow name says lint, but workflow does not run lint
Recommended next steps

Fix schema drift first
Because it blocks trust in whether features actually persist/work in production.
Repair AI task routing
Because AI is the product core and several flows look runtime-fragile.
Add real test coverage around auth + AI endpoints + DB writes
Because there is currently no safety net.
Make lint green and tighten types
Because current code quality debt is visible and broad.
Implement billing/entitlement + analytics
Because monetization and traction instrumentation are missing despite tier logic already existing.
SECTION 10: EXECUTIVE SUMMARY
Ready is a private React/TypeScript SaaS codebase for interview preparation and career coaching. The product is aimed at job seekers and combines onboarding, LinkedIn/portfolio assessment, mock interview practice, skills-gap analysis, negotiation coaching, rejection debriefing, readiness scoring, and a milestone handoff to the sister product Relevnt. The core user promise is clear in the code and docs: turn scattered career prep into a measurable “readiness” workflow.

Technically, the builder has assembled a credible modern stack: React 19 + Vite on the frontend, Netlify Functions as the backend surface, and Supabase for authentication, persistence, and RLS-based authorization. The codebase includes a real migration set, a multi-provider AI layer, typed frontend models, deployment config, CI, dark mode, reusable UI primitives, and prior security/performance/data-integrity audit artifacts. That signals strong product-building range, especially around rapid prototyping and architecture assembly.

The honest assessment is that Ready is an Alpha: more than a concept, clearly deployable, but not yet production-hardened. The biggest risks are not missing pages but execution gaps between layers: several feature flows reference schema elements not present in committed migrations, lint is failing, there are no tests, and some AI task wiring appears inconsistent enough to break at runtime. To reach the next milestone, the project needs schema consolidation, AI-path stabilization, automated tests, type/lint cleanup, and real monetization/analytics infrastructure.

## Text

AUDIT METADATA
Project: ready
Date: 2026-03-07
Agent: Advanced GitHub Copilot Coding Agent (underlying model/version not exposed in runtime)
Codebase access: full repo
Confidence level: medium-high — high for repository structure, dependencies, routes, migrations, scripts, CI, and asset/config facts; medium where runtime behavior depends on missing migrations or task-name mismatches that are visible in code but not executed end-to-end here.
Sections with gaps: 1, 2, 3, 5, 6, 8
Total files analyzed: 241

---
