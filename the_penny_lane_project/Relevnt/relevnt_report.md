## SECTION 1: PROJECT IDENTITY

| Item                       | Verified finding                                                                                                                                                                                                                                                                                                                                                                       |
| -------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1. Project name            | `relevnt-frontend` in [`package.json`](/Users/sarahsahl/Desktop/relevnt/package.json)                                                                                                                                                                                                                                                                                                  |
| 2. Repository URL          | `https://github.com/thepennylaneproject/Relevnt.git` from `git remote -v`                                                                                                                                                                                                                                                                                                              |
| 3. One-line description    | Exact: `"Relevnt - AI-powered career intelligence platform"` in [`package.json`](/Users/sarahsahl/Desktop/relevnt/package.json). Cleaner: AI-assisted job search, matching, application tracking, and career tooling.                                                                                                                                                                  |
| 4. Project status          | `Alpha`. Evidence: broad feature surface, working auth/routes/schema/tests/CI, but also placeholder Stripe IDs, deprecated/moved endpoints, missing root README, no verified production URL in committed config, and visible technical-debt cleanup work in recent commits.                                                                                                            |
| 5. First commit date       | `2025-11-14T13:27:21-06:00` (`Initial commit`)                                                                                                                                                                                                                                                                                                                                         |
| 6. Most recent commit date | `2026-03-06T18:56:30-06:00` (`Merge pull request #162...`)                                                                                                                                                                                                                                                                                                                             |
| 7. Total commits           | `678`                                                                                                                                                                                                                                                                                                                                                                                  |
| 8. Deployment status       | Netlify deployment is configured in [`netlify.toml`](/Users/sarahsahl/Desktop/relevnt/netlify.toml), with GitHub Actions CI in [`.github/workflows/ci.yml`](/Users/sarahsahl/Desktop/relevnt/.github/workflows/ci.yml). Scheduled serverless jobs are defined. A live deployment target is implied, but no committed `site_id`, Netlify app slug, or canonical main-app URL was found. |
| 9. Live URL(s)             | Verified app URL for this repo: not found. Comment/example-only URLs: `https://app.relevnt.com`, `https://dev.relevnt.com`, `https://*.relevnt.com` in [`netlify.toml`](/Users/sarahsahl/Desktop/relevnt/netlify.toml). Cross-product Ready URL: `https://relevnt.work/ready` in [`src/config/cross-product.ts`](/Users/sarahsahl/Desktop/relevnt/src/config/cross-product.ts).        |

Not found:

- Root `README.md` is absent.
- No committed production site URL for the main Relevnt app was found.

## SECTION 2: TECHNICAL ARCHITECTURE

### 1. Primary language(s) and frameworks

| Layer               | Verified stack                                                                                                                                                                                    |
| ------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Frontend            | TypeScript, React `^19.2.4`, React Router DOM `^7.13.1`, Vite `^7.3.1`                                                                                                                            |
| State/data          | React Context + hooks + TanStack React Query `^5.90.21`                                                                                                                                           |
| Backend             | Netlify Functions via `@netlify/functions ^5.1.0`                                                                                                                                                 |
| Database/auth       | Supabase JS `^2.98.0`, Postgres via Supabase                                                                                                                                                      |
| CLI/backend tooling | Supabase CLI `^2.77.0`, `pg ^8.12.0`                                                                                                                                                              |
| Styling             | CSS custom properties, shared CSS files, Tailwind config present in [`tailwind.config.ts`](/Users/sarahsahl/Desktop/relevnt/tailwind.config.ts) but `tailwindcss` is not listed in `package.json` |
| AI                  | OpenAI SDK `^6.27.0` plus custom HTTP providers for Anthropic, Google Gemini, DeepSeek, AIMLAPI, Brave, Tavily                                                                                    |

### 2. Full dependency list

| Group                        | Verified packages                                                                                                                                            |
| ---------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Core framework dependencies  | `react`, `react-dom`, `react-router-dom`                                                                                                                     |
| UI/styling libraries         | `clsx`, `lucide-react`                                                                                                                                       |
| State management             | `@tanstack/react-query`                                                                                                                                      |
| API/data layer               | `@supabase/supabase-js`, `pg`, `jsonwebtoken`, `fast-xml-parser`                                                                                             |
| AI/ML integrations           | `openai`                                                                                                                                                     |
| Authentication/authorization | `@supabase/supabase-js`, `jsonwebtoken`                                                                                                                      |
| Testing                      | `vitest`, `@testing-library/react`, `@testing-library/jest-dom`, `jsdom`, `@types/jest`                                                                      |
| Build tooling                | `vite`, `@vitejs/plugin-react`, `typescript`, `tsx`, `dotenv`, `netlify-cli`, `supabase`, `eslint`, `typescript-eslint`, `stylelint`, `husky`, `lint-staged` |
| Other notable dependencies   | `jspdf`, `mammoth`, `pdf-parse`, `pdfjs-dist`, `ts-jobspy`                                                                                                   |

### 3. Project structure

```text
.
├── .github/workflows        CI pipeline
├── audits/                  audit artifacts, prompts, synthesized findings/state
├── docs/ui/                 UI/design documentation
├── netlify/functions/       serverless API, ingestion jobs, admin endpoints, AI router
├── public/relevnt_final/    brand assets, logos, hero image, resume illustrations
├── scripts/                 audit, lint, schema, and ops scripts
├── src/
│   ├── assets/              app asset registry
│   ├── components/          UI and feature components
│   ├── config/              tiers, i18n, cross-product, Stripe config
│   ├── contexts/            auth and theme providers
│   ├── data/                static source data
│   ├── domain/              domain-layer utilities/types
│   ├── hooks/               frontend business logic/data hooks
│   ├── lib/                 shared client logic, scoring, analytics, API helpers
│   ├── pages/               route-level pages
│   ├── services/            app services and AI client wrappers
│   ├── styles/              global design system and page-level CSS
│   ├── themes/              asset/theme helpers
│   ├── types/               generated Supabase types and app types
│   └── ui/                  page-shell primitives
├── supabase/
│   ├── functions/           Supabase edge function(s)
│   ├── migrations/          active canonical migrations
│   ├── migrations_archive/  archived historical migrations
│   └── migrations_backup_20260129/ backup migration set
├── tests/                   SQL integrity test suite
└── dist/                    built frontend output
```

### 4. Architecture pattern

Verified pattern: a serverless JAMstack-style SPA with a single frontend and serverless backend, backed by Supabase/Postgres.

Verified request/data flow:

1. User interacts with React page/component in [`src/pages`](/Users/sarahsahl/Desktop/relevnt/src/pages).
2. Frontend hook either:
   - talks directly to Supabase from the browser for many CRUD operations, or
   - calls a Netlify Function with JWT and often CSRF protection for privileged/AI/admin flows.
3. Netlify Functions use authenticated or admin Supabase clients to read/write Postgres.
4. React Query/local component state updates UI.
5. Scheduled Netlify functions ingest jobs and compute summaries/metrics into Postgres tables consumed by the app/admin UI.

### 5. Database/storage layer

Verified sources:

- Current typed schema: [`src/types/supabase.ts`](/Users/sarahsahl/Desktop/relevnt/src/types/supabase.ts)
- Canonical migrations: [`supabase/migrations/`](/Users/sarahsahl/Desktop/relevnt/supabase/migrations)

Verified current typed table count: `106`.

Notes:

- RLS is broadly enabled in the baseline and follow-up migrations.
- The Supabase edge function [`supabase/functions/learning-search/index.ts`](/Users/sarahsahl/Desktop/relevnt/supabase/functions/learning-search/index.ts) references `learning_providers` and `learning_courses`, but those tables were not found in the current generated schema or active migration grep. That is a verifiable schema gap.

Compressed verified table inventory from `src/types/supabase.ts`:

```text
accredited_edu_domains: created_at, display_name, domain
admin_alerts: alert_type, created_at, description, id, is_dismissed, is_read, metadata, resolved_at, severity, source_slug, title, triggered_at
admin_audit_log: action, admin_id, context, created_at, description, error_message, id, ip_address, performed_at, status, user_agent
admin_config: created_at, description, key, updated_at, value
admin_metrics: active_users_month, analyses_run, avg_cost_per_analysis, avg_response_time_ms, churn_rate_percent, conversion_edu_to_paid, conversion_starter_to_pro, created_at, error_rate_percent, id, metric_date, mrr_premium, mrr_pro, mrr_total, new_signups_week, total_users, uptime_percent
aggregator_sources: base_url, calls_this_hour, created_at, hour_started_at, is_active, is_remote_only, name, rate_limit_per_hour, slug, supports_keyword_filter, supports_location_filter, updated_at
ai_cache: cache_key, created_at, expires_at, payload, quality, task_name, user_tier
ai_interactions: created_at, feature_type, feedback, id, input_summary, output_summary, processing_time_ms, tokens_used, user_id, user_rating
ai_invocations: cache_hit, cost_estimate, created_at, error_code, error_message, id, input_size, latency_ms, model, output_size, provider, quality, reason, success, task_name, tier, trace_id, user_id
ai_model_configs: cost_per_1k_tokens, created_at, feature, id, is_active, max_tokens, model_name, priority, provider, tier, updated_at
ai_usage_log: cost_estimate, created_at, id, metadata, provider, status, task_type, tokens_used, user_id
ai_usage_logs: cost_usd, created_at, id, model, provider, task, tier, tokens_input, tokens_output, user_id
ai_usage_tracking: cost, created_at, error_message, feature, id, job_id, model, provider, request_id, resume_id, tokens_used, user_id
analysis_explanations: analysis_type, created_at, criteria, examples, explanation, id, input_hash, model_used, reasoning, score, updated_at, user_id
analytics_daily_summaries: created_at, day, id, meta, metric_key, metric_value, updated_at
analytics_events: created_at, event_name, event_time, id, page_path, properties, referrer, session_id, user_agent, user_id
application_events: application_id, created_at, description, event_date, event_type, id, is_deleted, title, user_id
applications: ai_suggestions, application_source, applied_date, ats_optimization_applied, attempt_count, auto_apply_status, company, company_response_time, cover_letter, cover_letter_draft, cover_letter_final, created_at, deleted_at, estimated_probability, follow_up_date, id, interview_date, is_deleted, job_company_size, job_experience_level, job_id, job_industry, job_skills_required, last_attempt_at, last_error, last_status_update, location, metadata, negotiation_notes, negotiation_strategy, notes, offer_date, offer_details, persona_id, position, qa_answers, ranking_explanation, recruiter_email, recruiter_name, recruiter_phone, rejection_analysis, response_deadline, response_received, resume_id, resume_snapshot, rule_id, salary_expectation, status, submission_method, target_salary_max, target_salary_min, trace_id, updated_at, user_id
ats_detection_queue: company_id, confidence_score, created_at, detected_ats, detected_slug, discovered_url, discovery_query, discovery_source, domain, error_message, id, last_checked_at, status
ats_detection_cache: company, confidence_score, detected_at, detection_method, domain, id, ats_type
audit_logs: admin_id, change_reason, created_at, id, ip_address, new_value, old_value, operation_type, request_metadata, resource_id, resource_name, resource_type, updated_at, user_agent, user_id
auto_apply_logs: application_id, artifacts, attempt_count, created_at, error_message, id, job_id, persona_id, queue_item_id, rule_id, status, submission_url, trace_id, updated_at, user_id, withdrawn_at
auto_apply_queue: attempt_count, created_at, error_class, id, job_id, metadata, persona_id, priority, processed_at, rule_id, scheduled_for, status, user_id
auto_apply_rules: active_days, created_at, enabled, exclude_companies, failed_applications, id, include_only_companies, last_run_at, match_score_threshold, max_applications_per_week, name, persona_id, require_all_keywords, successful_applications, total_applications, updated_at, user_id
auto_apply_settings: apply_only_canonical, created_at, enabled, max_per_week, min_match_score, min_salary, mode, require_values_alignment, updated_at, user_id
cache_invalidation_log: cache_key, cache_type, created_at, id, invalidated_at, reason, user_id
career_narratives: created_at, future_vision, id, origin_story, pivot_explanation, updated_at, user_id, value_proposition, voice_settings
career_profiles: created_at, id, profile, updated_at, user_id
career_tracks: created_at, description, id, is_default, min_salary, name, preferred_locations, primary_resume_id, target_titles, updated_at, user_id, work_mode
collection_jobs: added_at, collection_id, id, job_id
companies: aliases, ashby_slug, ats_confidence_score, ats_detected_at, ats_type, breezyhr_slug, canonical_name, careers_page_url, created_at, discovered_at, discovered_via, discovery_metadata, domain, employee_count, founding_year, funding_stage, greenhouse_board_token, growth_score, id, industry, is_active, jazzhr_slug, job_creation_velocity, last_synced_at, last_synced_at_greenhouse, last_synced_at_lever, last_synced_platforms, lever_slug, name, personio_slug, priority_tier, recruitee_slug, smartrecruiters_slug, sync_frequency_hours, updated_at, verified_at, workday_tenant_url
company_targets: avg_jobs_per_run, company_id, company_slug, consecutive_empty_runs, created_at, fail_count, id, last_error, last_success_at, min_interval_minutes, new_jobs_last, next_allowed_at, platform, priority, status, total_jobs_found, updated_at
constraint_violations: attempted_values, constraint_name, created_at, id, table_name, user_id, violation_message
contacts: company, created_at, email, id, last_contacted, linkedin_url, name, notes, phone, position, relationship, tags, updated_at, user_id
cover_letters: ai_generated, application_id, company_name, content, created_at, id, job_description, job_id, resume_id, template_used, title, updated_at, user_id
critical_operations_audit: action, created_at, entity_id, entity_type, error_message, id, ip_address, metadata, operation_type, status, user_agent, user_id
daily_ingestion_metrics: avg_duration_seconds, created_at, date, duplicate_rate_percent, sources_with_errors, success_rate, total_duplicates, total_failed, total_inserted, total_runs
data_cleanup_audit: cleaned_at, created_at, field_name, id, new_value, old_value, reason, record_id, table_name
discovery_runs: completed_at, created_at, duration_ms, errors, id, run_id, sources, started_at, stats, status
failure_logs: created_at, error_code, error_message, error_stack, id, is_resolved, operation_type, request_metadata, resolution_note, resolved_at, resource_id, resource_type, severity, updated_at, user_id
feedback_signals: company_size, created_at, feedback_type, id, industry, job_id, location, persona_id, remote_type, user_id
ingestion_activity_log: created_at, duration_seconds, error_message, finished_at, id, progress_percent, run_id, sources_requested, started_at, status, total_duplicates, total_failed, total_inserted, trigger_type
ingestion_healing_log: attempted_at, created_at, failure_type, healing_action, healing_result, id, meta, original_error, resolved_at, run_id, source
ingestion_runs: batch_id, completed_at, duplicates_by_primary, duplicates_by_secondary, duplicates_by_tertiary, duplicates_skipped, error_message, id, jobs_batch_deduped, jobs_failed, jobs_fetched, jobs_inserted, jobs_normalized, jobs_unchanged, jobs_updated, metadata, source_slug, started_at, status
interview_practice_sessions: application_id, created_at, id, interview_prep_id, job_id, overall_feedback, practice_data, questions, status, updated_at, user_id
interview_prep: ai_feedback, application_id, company, created_at, id, interview_date, interview_type, position, practice_sessions, questions, updated_at, user_id
interview_sessions: created_at, feedback, id, job_id, question, score, user_answer, user_id
job_application_artifacts: ai_trace_id, artifact_type, content, format, generated_at, id, job_id, metadata, persona_id, user_id
job_applications: applied_at, created_at, details, id, job_id, match_score, mode, source, status, updated_at, user_id
job_collections: color, created_at, id, job_count, name, updated_at, user_id
job_cross_references: canonical_job_id, company_name, created_at, job_ids_by_source, job_title, location, matched_confidence, updated_at
job_duplicates: canonical_job_id, confidence, created_at, duplicate_job_ids, matched_at, sources
job_ingestion_calls: called_at, id, result_count, signature, source
job_ingestion_run_sources: batch_deduped_count, created_at, cursor_in, cursor_out, duplicate_count, error_message, finished_at, freshness_ratio, http_status, id, inserted_count, invalid_count, noop_count, normalized_count, page_end, page_start, run_id, source, stale_filtered_count, started_at, status, updated_count
job_ingestion_runs: created_at, error_summary, finished_at, id, meta, sources_requested, started_at, status, total_batch_deduped, total_duplicates, total_failed_sources, total_inserted, total_invalid, total_normalized, total_stale_filtered, triggered_by
job_ingestion_state: consecutive_high_noop_runs, cooldown_multiplier, cursor, last_inserted_count, last_noop_count, last_noop_ratio, last_run_at, last_updated_count, source
job_interaction_patterns: created_at, id, interaction_type, job_id, match_factors, match_score, persona_id, user_id
job_matches: ats_likelihood, calculated_at, created_at, is_dismissed, is_score_stale, job_id, match_score, probability_estimate, profile_fingerprint, reasons, scores_calculated_at, scoring_version, status, user_id
job_preferences: allowed_timezones, auto_apply_max_apps_per_day, auto_apply_min_match_score, avoid_keywords, created_at, diversity_signal_weight, enable_auto_apply, exclude_companies, exclude_contract_types, exclude_titles, id, include_keywords, max_salary, min_salary, pref_accommodations, pref_inclusive_language, pref_no_degree_req, pref_non_linear_ok, pref_salary_transparency, pref_visa_sponsorship, preferred_locations, primary_title, related_titles, remote_preference, salary_currency, salary_unit, seniority_levels, target_functions, updated_at, user_id
job_search_profiles: created_at, enabled, id, keywords, last_run_at, location, priority, source
job_source_health: auto_heal_enabled, consecutive_failures, failure_batch_context, failure_stage, failure_type, heal_attempts_24h, is_degraded, is_healthy, last_checked_at, last_counts, last_error_at, last_failure_reason, last_heal_attempt_at, last_run_at, last_success_at, max_age_days, median_job_age_days, retry_in_progress, retry_started_at, source, source_mode, success_count_24h, updated_at
job_source_performance: avg_match_score, created_at, id, last_sync, source_name, total_jobs_indexed, total_matches_generated, updated_at, user_satisfaction_score
job_sources: api_key_required, auth_mode, created_at, enabled, endpoint_url, id, last_error, last_fetch, last_sync, metadata, mode, name, public_key, rate_limit_per_minute, secret_key, slug, source_key, update_frequency, updated_at, website_url
jobs: ats_type, benefits, company, company_id, company_logo_url, company_size, competitiveness_level, created_at, cross_source_dedup_key, dedup_key, description, education_level, effective_posted_date, employment_type, enrichment_confidence, experience_years_max, experience_years_min, external_id, external_job_id, external_source, external_url, extracted_structure, id, industry, is_active, is_direct, is_official, job_fingerprint, job_fingerprint_secondary, job_fingerprint_tertiary, job_type, keywords, last_seen_at, location, match_reasons, match_score, match_score_calculated_at, normalized_company, normalized_location, normalized_title, original_posting_url, posted_date, preferred_skills, probability_estimate, ranking_score, raw, raw_payload, remote_type, required_skills, requirements, responsibilities, salary_max, salary_min, salary_range, saved_date, seniority_level, source, source_id, source_slug, source_url, status, title, updated_at, url, url_enriched, url_enrichment_confidence, url_enrichment_method, user_id
jobs_history: archive_reason, archived_at, ats_type, benefits, company, company_id, company_logo_url, company_size, competitiveness_level, created_at, cross_source_dedup_key, dedup_key, description, education_level, effective_posted_date, employment_type, enrichment_confidence, experience_years_max, experience_years_min, external_id, external_job_id, external_source, external_url, extracted_structure, id, industry, is_active, is_direct, is_official, job_fingerprint, job_fingerprint_secondary, job_fingerprint_tertiary, job_type, keywords, last_seen_at, location, match_reasons, match_score, match_score_calculated_at, normalized_company, normalized_location, normalized_title, original_posting_url, posted_date, preferred_skills, probability_estimate, ranking_score, raw, raw_payload, remote_type, required_skills, requirements, responsibilities, salary_max, salary_min, salary_range, saved_date, seniority_level, source, source_id, source_slug, source_url, status, title, updated_at, url, url_enriched, url_enrichment_confidence, url_enrichment_method, user_id
linkedin_profiles: analysis_results, created_at, id, is_public, linkedin_url, profile_data, share_token, updated_at, user_id
maintenance_jobs: created_at, description, enabled, function_name, id, job_name, job_type, last_run_at, last_run_duration_ms, last_run_status, next_run_at, schedule_cron, updated_at
maintenance_log: created_at, duration_ms, error_message, execution_completed_at, execution_started_at, id, job_name, records_affected, status
maintenance_runs: details, duration_ms, error, id, job_name, reason, rows_affected, run_at, status
market_snapshots: accommodations_pct, ats_bypass_pct, ats_detected_pct, classifier_version, enterprise_pct, epistemology_community_count, epistemology_corporate_count, epistemology_grey_count, epistemology_smb_count, epistemology_unknown_count, epistemology_venture_count, herfindahl_index, id, inclusive_language_pct, large_pct, medium_pct, no_degree_req_pct, non_linear_ok_pct, percent_classifiable, percent_with_description, salary_banded_pct, small_pct, snapshot_date, sources_ingested, startup_pct, taken_at, top_100_employers_coverage_pct, total_jobs_in_window, unique_employers, unique_employers_count, visa_sponsorship_pct
networking_contacts: company, created_at, email, id, linkedin_url, name, notes, role, status, updated_at, user_id
notifications: created_at, id, idempotency_key, is_read, link, message, title, type, user_id
observed_titles: created_at, id, is_in_search_deck, last_seen_at, normalized_title, occurrence_count, raw_title
outreach_logs: contact_id, id, message_content, method, response_received, sent_at, user_id
outreach_templates: content, id, name, type, user_id
persona_preferences: company_size, created_at, excluded_companies, growth_focus, id, industries, job_title_keywords, locations, max_salary, min_salary, mission_values, nice_to_have_skills, persona_id, remote_preference, required_skills, updated_at, voice_conciseness, voice_formality, voice_playfulness
portfolio_analyses: analysis_results, created_at, id, is_public, portfolio_url, share_token, updated_at, user_id
provider_health: circuit_open, created_at, failures, id, last_failed, last_reset, opened_at, provider_name, updated_at
preferences: created_at, deal_breakers, email_notifications, id, max_salary, min_salary, nice_to_have_skills, notification_frequency, preferred_companies, preferred_job_titles, preferred_job_types, preferred_locations, remote_preference, required_skills, updated_at, user_id, willing_to_relocate
profiles: admin_level, auto_apply_active, avatar_url, career_narrative_future, career_narrative_origin, career_narrative_pivot, career_narrative_value, career_priorities, created_at, current_role_title, edu_verified, email, enable_experimental_features, full_name, id, is_admin, last_narrative_generated_at, layout_density, location, matches_last_recalculated_at, max_salary, min_salary, notif_application_updates, notif_high_match, notif_summary, notif_system, notif_weekly_digest, onboarding_completed, onboarding_dismissed, onboarding_state, onboarding_step, plan_tier, preferred_name, theme_preference, tier, timezone, updated_at, use_data_for_recommendations, voice_conciseness, voice_custom_sample, voice_formality, voice_playfulness, voice_preset
query_performance_log: duration_ms, estimated_cost, function_name, id, is_slow, operation, row_count, table_name, timestamp
rate_limit_events: function_name, id, key, timestamp, user_id
ready_profiles: assessments_completed, created_at, display_name, focus_areas, goal, headline, id, last_practice_date, readiness_score, updated_at
relevance_tuner_settings: created_at, id, industry_weight, is_default, location_weight, name, remote_weight, salary_weight, skill_weight, updated_at, user_id
resumes: ats_score, ats_suggestions, cached_keywords, certifications, created_at, deleted_at, education, file_name, file_size_bytes, id, is_default, is_deleted, keywords, keywords_extracted_at, languages, last_optimization_date, mime_type, optimization_history, parsed_fields, parsed_text, personal_info, projects, skills, summary, title, updated_at, user_id, version_number, work_experience
resume_versions_history: captured_at, id, parsed_fields, resume_id, title, updated_at, user_id, version_number
saved_jobs: id, job_id, list, saved_at, user_id
search_queue: avg_jobs_per_run, consecutive_empty_runs, created_at, id, keywords, last_result_count, last_run_at, location, min_interval_minutes, next_run_after, params, priority, run_count, source_slug, status, total_jobs_found, updated_at
search_slices: consecutive_empty_runs, created_at, fail_count, id, last_error, last_success_at, min_interval_minutes, new_jobs_last, next_allowed_at, params_json, query_hash, result_count_last, source, status, updated_at
signal_classifications: classification_model_version, classified_at, confidence, detected_value, evidence, id, job_id, signal_type
skill_gap_analyses: action_plan, created_at, current_skills, gaps, id, status, strengths, target_role, updated_at, user_id
skills_library: category, certifications, created_at, display_name, examples, id, last_used, proficiency_level, skill_name, slug, updated_at, used_in_resumes, user_id, years_of_experience
source_performance_metrics: avg_jobs_per_run_30d, avg_jobs_per_run_7d, consecutive_failures, failed_runs_30d, failed_runs_7d, health_factors, health_score, is_degraded, last_error, last_error_at, last_run_at, source_slug, success_rate_30d, success_rate_7d, total_runs_30d, total_runs_7d, updated_at
source_rate_limits: cooldown_minutes, is_active, max_per_hour, source_slug, updated_at
strategic_pivot_reports: created_at, generated_at, id, insights, interview_rate, offer_rate, period_end, period_start, recommendations, recommendations_applied, recommendations_dismissed, response_rate, total_applications, total_interviews, total_offers, total_rejections, total_responses, updated_at, user_id
title_families: created_at, family_name, id, is_active, keywords, seniority_variants
user_alert_history: alert_type, id, job_id, sent_at, user_id
user_keywords: keywords, updated_at, user_id
user_match_preferences: created_at, location_mode, min_salary_local, min_salary_relocate, min_salary_remote, updated_at, user_id, weight_growth, weight_location, weight_mission, weight_remote, weight_salary
user_pattern_insights: created_at, expires_at, id, insight_data, insight_message, insight_title, insight_type, is_dismissed, priority, user_id
user_personas: created_at, description, focus_metadata, id, is_active, name, resume_id, updated_at, user_id
user_professional_profiles: career_data_raw, created_at, domestic_authorized, earliest_start_raw, evergreen_strengths_polished, evergreen_strengths_raw, evergreen_why_polished, evergreen_why_raw, headline_polished, headline_raw, international_interest, international_requires_sponsorship, links, links_raw, needs_sponsorship, relocate_notes, relocate_preference, summary_polished, summary_raw, target_roles, target_roles_raw, top_skills, top_skills_raw, travel_preference, updated_at, user_id, work_auth_raw, work_types
user_search_interests: created_at, id, keywords, last_seeded_at, location, updated_at, user_count, weight
user_skill_preferences: avoid_skills, created_at, focus_skills, id, learning_style, updated_at, user_id
user_skills: created_at, id, proficiency_level, skill_name, source, user_id
voice_profiles: created_at, id, preset_name, settings, updated_at, user_id
```

### 6. API layer

Verified serverless routes in [`netlify/functions`](/Users/sarahsahl/Desktop/relevnt/netlify/functions):

| Route                                     | Method(s)                  | Purpose                                            | Auth                    |
| ----------------------------------------- | -------------------------- | -------------------------------------------------- | ----------------------- |
| `/.netlify/functions/ai`                  | `POST`                     | AI task router/execution                           | Yes                     |
| `/.netlify/functions/application_helper`  | `POST`                     | AI/form answer helper for applications             | Yes                     |
| `/.netlify/functions/auth-logout`         | `POST`                     | server-side logout/cleanup                         | Yes                     |
| `/.netlify/functions/create_profile`      | `POST`                     | create profile after signup                        | Yes                     |
| `/.netlify/functions/csrf_token`          | `GET`                      | issue/sync CSRF token                              | Yes                     |
| `/.netlify/functions/get_market_trends`   | `GET`                      | derive target-title skill trends from jobs         | Yes                     |
| `/.netlify/functions/get_matched_jobs`    | `GET`                      | fetch matched jobs for current user/target         | Yes                     |
| `/.netlify/functions/health`              | `GET`                      | runtime health check                               | No                      |
| `/.netlify/functions/match_jobs`          | `POST`                     | run job matching                                   | Yes                     |
| `/.netlify/functions/personas`            | `GET, POST, PATCH, DELETE` | CRUD for user targets/personas                     | Yes; CSRF on writes     |
| `/.netlify/functions/recalculate-matches` | `POST`                     | trigger match recalculation                        | Yes                     |
| `/.netlify/functions/search_jobs`         | `GET, POST`                | jobs-table search with filters/pagination          | No auth check found     |
| `/.netlify/functions/track_event`         | `POST`                     | batch analytics event ingestion                    | Optional auth           |
| `/.netlify/functions/tuner-settings`      | `GET, POST, PATCH, DELETE` | CRUD relevance tuner settings                      | Yes; CSRF on writes     |
| `/.netlify/functions/upload_resume`       | `POST`                     | resume upload/parsing pipeline entry               | Yes                     |
| `/.netlify/functions/verify-edu-email`    | `POST`                     | verify `.edu` email and upgrade to Pro if eligible | Yes; CSRF; rate-limited |
| `/.netlify/functions/whoami`              | `GET`                      | identity/debug endpoint                            | Yes                     |

Admin endpoints:

| Route                                             | Method(s)   | Purpose                                | Auth                      |
| ------------------------------------------------- | ----------- | -------------------------------------- | ------------------------- |
| `/.netlify/functions/admin_company_targets`       | `GET, POST` | inspect/update company target rotation | Admin                     |
| `/.netlify/functions/admin_config`                | `GET, POST` | inspect/update admin config            | Admin                     |
| `/.netlify/functions/admin_ingest_trigger`        | `POST`      | trigger ingestion manually             | Admin                     |
| `/.netlify/functions/admin_ingestion_health`      | `GET`       | ingestion health, source health, runs  | Admin                     |
| `/.netlify/functions/admin_ingestion_runs`        | `GET`       | ingestion run history                  | Admin                     |
| `/.netlify/functions/admin_log_ingestion`         | `POST`      | log ingestion event                    | Admin/auth signal present |
| `/.netlify/functions/admin_logs/failures`         | `GET`       | query failure logs                     | Admin                     |
| `/.netlify/functions/admin_logs/failures/summary` | `GET`       | summarize failure logs                 | Admin                     |
| `/.netlify/functions/admin_logs/audit`            | `GET`       | query audit logs                       | Admin                     |
| `/.netlify/functions/admin_logs/audit/summary`    | `GET`       | summarize audit logs                   | Admin                     |
| `/.netlify/functions/admin_market_metrics`        | `GET`       | market snapshots/trends/dashboard data | Admin                     |
| `/.netlify/functions/admin_monitoring_stats`      | `GET`       | monitoring stats                       | Admin                     |
| `/.netlify/functions/admin_search_slices`         | `GET, POST` | inspect/update search rotation slices  | Admin                     |

Scheduled/background/job endpoints:

```text
daily_summaries
job_alerts
generate_market_snapshot
ingest_premium_sources
ingest_lever-background
ingestion_healer-background
maintenance-indexing
maintenance-archival
maintenance-monitoring
jobspy_ingest-background
adjust-intervals
scrape-ashby
scrape-smartrecruiters
scrape-workday
scrape-recruitee
scrape-breezyhr
scrape-jazzhr
scrape-personio
detect-ats
promote-titles
generate-search-slices
ingest_aggregators
ingest_jobs
ingest_jobs_worker-background
ingest_remaining_sources
search_queue_cron
cleanup_jobs-background
cleanup-deleted-applications
build_auto_apply_queue
auto_apply_retry_handler
auto_apply_mark_submitted
auto_apply_mark_failed
```

Deprecated/moved endpoints returning `410` or equivalent “moved to Ready” response:

```text
applications
jobs
resumes
user-profile
ai_ops
analyze_rejection
discover_companies
get_application_performance
interview_evaluate
interview_prepare
linkedin_profile_analyze
portfolio_analyze
admin_source_config
```

Supabase edge function:

- `supabase/functions/learning-search`:
  - method: request-body JSON handler
  - purpose: search learning providers and upsert course data
  - auth: no user auth check in inspected code
  - gap: references tables not found in current typed schema

### 7. External service integrations

Verified in code/env references:

| Service                                                                                   | Use                                                       |
| ----------------------------------------------------------------------------------------- | --------------------------------------------------------- |
| Supabase                                                                                  | Postgres, Auth, Realtime, RLS-backed data access          |
| Netlify                                                                                   | serverless runtime and scheduled jobs                     |
| OpenAI                                                                                    | AI provider in `netlify/functions/ai/providers/openai.ts` |
| Anthropic                                                                                 | AI provider via custom HTTP client                        |
| Google Gemini                                                                             | AI provider via custom HTTP client                        |
| DeepSeek                                                                                  | AI provider via custom HTTP client                        |
| AIMLAPI                                                                                   | lower-cost AI routing option                              |
| Brave Search API                                                                          | posting finder / search provider                          |
| Tavily                                                                                    | search provider                                           |
| Stripe                                                                                    | webhook-based subscription/tier updates                   |
| Slack webhook                                                                             | alerts                                                    |
| Generic webhook (`ALERT_WEBHOOK_URL`)                                                     | operational alerting                                      |
| Adzuna                                                                                    | job ingestion                                             |
| Careerjet                                                                                 | job ingestion                                             |
| CareerOneStop                                                                             | job ingestion                                             |
| Crunchbase                                                                                | company discovery                                         |
| Fantastic Jobs                                                                            | job ingestion                                             |
| Findwork                                                                                  | job ingestion                                             |
| Google API                                                                                | job/source integration signal present                     |
| Jooble                                                                                    | job ingestion                                             |
| Reed                                                                                      | job ingestion                                             |
| TheirStack                                                                                | job ingestion                                             |
| The Muse                                                                                  | job ingestion                                             |
| USAJobs                                                                                   | job ingestion                                             |
| WhatJobs                                                                                  | job ingestion                                             |
| Greenhouse, Lever, Ashby, SmartRecruiters, Workday, Recruitee, BreezyHR, JazzHR, Personio | ATS scraping/detection/ingestion                          |
| Coursera, edX, freeCodeCamp                                                               | referenced by Supabase edge learning-search function      |

### 8. AI/ML components

Verified providers/models:

- OpenAI `gpt-4o-mini`, `gpt-4o`
- Anthropic `claude-3-5-sonnet-20241022`
- Google `gemini-1.5-flash-latest`
- DeepSeek `deepseek-chat`
- AIMLAPI-backed models
- Brave/Tavily as search tools in AI workflows

Verified task contracts in [`src/lib/ai/tasks.ts`](/Users/sarahsahl/Desktop/relevnt/src/lib/ai/tasks.ts):

- `resume_extract_structured`
- `resume_ats_analysis`
- `resume_bullet_rewrite`
- `cover_letter_generate`
- `job_match_explanation`
- `keyword_extraction`
- `coach_chat_tip`
- `general_chat_assist`
- `application_question_answer`
- `linkedin_profile_analysis`
- `portfolio_analysis`
- `interview_prepare`
- `interview_evaluate`
- `strategic_insights_generate`

Verified prompt behavior in [`netlify/functions/ai/prompting.ts`](/Users/sarahsahl/Desktop/relevnt/netlify/functions/ai/prompting.ts):

- system prompt forces task contract adherence
- JSON tasks get schema-oriented JSON-only prompt
- user payload is wrapped as `UNTRUSTED_USER_PAYLOAD_*`
- explicit prompt-injection resistance instructions are present

Verified AI output processing:

- JSON normalization and schema enforcement in the AI router
- cache layer for selected tasks
- tier/quality clamping
- usage caps and monthly cost guardrails
- telemetry stored in AI/audit tables
- frontend surfaces AI through hooks like [`useAI.ts`](/Users/sarahsahl/Desktop/relevnt/src/hooks/useAI.ts) and [`useAITask.ts`](/Users/sarahsahl/Desktop/relevnt/src/hooks/useAITask.ts)

### 9. Authentication and authorization model

Verified model:

- Login/signup: Supabase email/password auth in [`AuthContext.tsx`](/Users/sarahsahl/Desktop/relevnt/src/contexts/AuthContext.tsx)
- Session persistence: Supabase client with `persistSession` and `autoRefreshToken`
- Profile bootstrap: `create_profile` function after signup
- Protected routes: `AuthGuard`
- CSRF: token bootstrap/sync for authenticated writes
- Admin authorization:
  - frontend UI gate: `profiles.is_admin === true` or `profiles.admin_level === 'super'`
  - backend: independent admin verification in Netlify Functions
- Product tiers:
  - `starter`
  - `pro`
  - `premium`
- Education flow:
  - `.edu` verification can upgrade eligible users to Pro
- Usage/plan enforcement:
  - AI caps
  - resume/saved-job/application limits
  - auto-apply weekly limits
  - backend tier-guard triggers on settings/preferences writes

### 10. Environment variables

Grouped verified env var names:

```text
Frontend/Supabase:
VITE_SUPABASE_URL
VITE_SUPABASE_ANON_KEY
VITE_FUNCTIONS_BASE_URL
VITE_FRONTEND_URL
VITE_HIDE_SOURCES
VITE_ENABLE_BULLET_BANK
VITE_SENTRY_DSN

Supabase/server:
SUPABASE_URL
SUPABASE_SERVICE_ROLE_KEY
SUPABASE_SERVICE_KEY
SUPABASE_JWT_SECRET
SUPABASE_ANON_KEY
SUPABASE_PROJECT_REF
SUPABASE_DB_URL
SUPABASE_DB_CONNECTION_STRING
SUPABASE_DB_NAME
SUPABASE_METRICS_URL
DATABASE_URL
JWT_SECRET

AI/providers:
OPENAI_API_KEY
ANTHROPIC_API_KEY
DEEPSEEK_API_KEY
AIMLAPI_API_KEY
TAVILY_API_KEY
BRAVE_API_KEY
GOOGLE_API_KEY
AI_MONTHLY_COST_LIMIT_USD
AI_MAX_ATTEMPTS

Job-source APIs:
ADZUNA_APP_ID
ADZUNA_APP_KEY
CAREERJET_API_KEY
CAREERONESTOP_API_KEY
CAREERONESTOP_TOKEN
CAREERONESTOP_USER_ID
CRUNCHBASE_API_KEY
FANTASTIC_JOBS_API_KEY
FINDWORK_API_KEY
JOOBLE_API_KEY
REED_API_KEY
THEIRSTACK_API_KEY
THEIRSTACK_MAX_RESULTS_PER_RUN
THEMUSE_API_KEY
USAJOBS_API_KEY
USAJOBS_USER_AGENT
WHATJOBS_API_KEY

Ingestion controls:
ENABLE_SOURCE_GREENHOUSE
ENABLE_SOURCE_LEVER
ENABLE_SOURCE_RSS
ENABLE_SOURCE_CAREERONESTOP
GREENHOUSE_MAX_BOARDS_PER_RUN
GREENHOUSE_MAX_JOBS_PER_BOARD
LEVER_MAX_COMPANIES_PER_RUN
RSS_MAX_FEEDS_PER_RUN
MAX_COMPANY_TARGETS_PER_RUN
MAX_SEARCH_SLICES_PER_RUN
QUARANTINE_SOURCES

Security/session/CORS:
CSRF_SECRET
CORS_ALLOWED_ORIGINS
STRIPE_WEBHOOK_SECRET

Ops/maintenance/monitoring:
ARCHIVE_BATCH_SIZE
ARCHIVE_BEFORE_DAYS
ARCHIVE_DRY_RUN
ARCHIVE_BY
ARCHIVE_HISTORY_TABLE
ARCHIVE_SCHEMA
ARCHIVE_TABLE
MAINTENANCE_DISABLED
MAINTENANCE_DST_GATE
MAINTENANCE_MONITOR_DST_GATE
MAINTENANCE_LOCK_TIMEOUT_MS
MAINTENANCE_STATEMENT_TIMEOUT_MS
ALERT_WEBHOOK_URL
SLACK_WEBHOOK_URL
METRICS_CACHE_PATH
DISK_IO_READ_BLOCKS_PER_MIN_THRESHOLD
DISK_IO_WRITE_BLOCKS_PER_MIN_THRESHOLD
INDEX_AGENT_APPLY
INDEX_AGENT_MAX_CREATE
INDEX_AGENT_MIN_CALLS
INDEX_AGENT_MIN_MEAN_MS
INDEX_AGENT_SCHEMA
INDEX_AGENT_TABLE

Deployment/runtime/debug:
ENVIRONMENT
APP_VERSION
SERVICE_NAME
NODE_ENV
CI
DD_TRACE_ENABLED
AWS_REGION
DEBUG_JOB_ENRICHER
DEBUG_KEYWORD_EXTRACTION
DEBUG_PROFILE_AGGREGATOR
RUN_AI_INTEGRATION_TESTS
RUN_DB_TESTS
RUN_DB_TESTS_RPC
AUDIT_BASE_REF
AUDIT_CYCLE_START
BATCH_UPSERT_DRY_RUN
BATCH_UPSERT_SIZE
```

## SECTION 3: FEATURE INVENTORY

| Feature                                | User-facing description                                                          | Completeness | Key files                                                                                                                                                                                                                                                                                                                                                                                                                                                              | Dependencies                                        |
| -------------------------------------- | -------------------------------------------------------------------------------- | ------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------- |
| Authentication                         | Sign up, log in, log out, reset password, session expiry recovery                | Functional   | [`AuthContext.tsx`](/Users/sarahsahl/Desktop/relevnt/src/contexts/AuthContext.tsx), [`SignupPage.tsx`](/Users/sarahsahl/Desktop/relevnt/src/pages/SignupPage.tsx), [`LoginPage.tsx`](/Users/sarahsahl/Desktop/relevnt/src/pages/LoginPage.tsx), [`auth-logout.ts`](/Users/sarahsahl/Desktop/relevnt/netlify/functions/auth-logout.ts)                                                                                                                                  | Supabase Auth, profiles                             |
| Profile bootstrap                      | Automatically create/load user profile after signup/login                        | Functional   | [`create_profile.ts`](/Users/sarahsahl/Desktop/relevnt/netlify/functions/create_profile.ts), [`AuthContext.tsx`](/Users/sarahsahl/Desktop/relevnt/src/contexts/AuthContext.tsx)                                                                                                                                                                                                                                                                                        | Auth                                                |
| Onboarding target wizard               | Create first job target with titles, location, salary, skills                    | Functional   | [`OnboardingWizard.tsx`](/Users/sarahsahl/Desktop/relevnt/src/components/onboarding/OnboardingWizard.tsx), [`useTargets.ts`](/Users/sarahsahl/Desktop/relevnt/src/hooks/useTargets.ts)                                                                                                                                                                                                                                                                                 | Auth, personas/targets                              |
| Dashboard                              | “What should I do today?” next-step dashboard                                    | Functional   | [`DashboardPage.tsx`](/Users/sarahsahl/Desktop/relevnt/src/pages/DashboardPage.tsx), [`useApplications.ts`](/Users/sarahsahl/Desktop/relevnt/src/hooks/useApplications.ts), [`useResumes.ts`](/Users/sarahsahl/Desktop/relevnt/src/hooks/useResumes.ts), [`useMatchedJobs.ts`](/Users/sarahsahl/Desktop/relevnt/src/hooks/useMatchedJobs.ts)                                                                                                                           | Auth, jobs, resumes, applications, targets          |
| Jobs discovery and matching            | Browse jobs, filter, see personalized matches, ATS likelihood, quick apply/save  | Functional   | [`JobsPage.tsx`](/Users/sarahsahl/Desktop/relevnt/src/pages/JobsPage.tsx), [`useMatchedJobs.ts`](/Users/sarahsahl/Desktop/relevnt/src/hooks/useMatchedJobs.ts), [`get_matched_jobs.ts`](/Users/sarahsahl/Desktop/relevnt/netlify/functions/get_matched_jobs.ts), [`search_jobs.ts`](/Users/sarahsahl/Desktop/relevnt/netlify/functions/search_jobs.ts)                                                                                                                 | Targets, job_preferences, jobs, job_matches         |
| Saved jobs and collections             | Save jobs, organize by list/collection, manage collections                       | Functional   | [`SavedJobsPage.tsx`](/Users/sarahsahl/Desktop/relevnt/src/pages/SavedJobsPage.tsx), [`useSavedJobs.ts`](/Users/sarahsahl/Desktop/relevnt/src/hooks/useSavedJobs.ts), [`useJobCollections.ts`](/Users/sarahsahl/Desktop/relevnt/src/hooks/useJobCollections.ts)                                                                                                                                                                                                        | Jobs, saved_jobs, job_collections                   |
| Application tracking                   | Track applications, status transitions, undo delete, trash, events               | Functional   | [`ApplicationsPage.tsx`](/Users/sarahsahl/Desktop/relevnt/src/pages/ApplicationsPage.tsx), [`TrashPage.tsx`](/Users/sarahsahl/Desktop/relevnt/src/pages/TrashPage.tsx), [`useApplications.ts`](/Users/sarahsahl/Desktop/relevnt/src/hooks/useApplications.ts)                                                                                                                                                                                                          | Applications, application_events                    |
| Documents/resume workspace             | Manage resumes and cover letters in rail/canvas editor                           | Functional   | [`DocumentsPage.tsx`](/Users/sarahsahl/Desktop/relevnt/src/pages/DocumentsPage.tsx), [`useResumeBuilder.ts`](/Users/sarahsahl/Desktop/relevnt/src/hooks/useResumeBuilder.ts), [`useResumes.ts`](/Users/sarahsahl/Desktop/relevnt/src/hooks/useResumes.ts), [`DocumentsTrashPage.tsx`](/Users/sarahsahl/Desktop/relevnt/src/pages/DocumentsTrashPage.tsx)                                                                                                               | Resumes, cover_letters                              |
| Resume upload and parsing              | Upload resumes and extract structured content                                    | Functional   | [`useResumes.ts`](/Users/sarahsahl/Desktop/relevnt/src/hooks/useResumes.ts), [`upload_resume.ts`](/Users/sarahsahl/Desktop/relevnt/netlify/functions/upload_resume.ts), [`resumeParsing.ts`](/Users/sarahsahl/Desktop/relevnt/netlify/functions/utils/resumeParsing.ts)                                                                                                                                                                                                | AI router, resumes                                  |
| Resume tailoring / ATS / cover letters | AI-powered rewrite, ATS suggestions, cover-letter generation                     | Functional   | [`useAITask.ts`](/Users/sarahsahl/Desktop/relevnt/src/hooks/useAITask.ts), [`ai.ts`](/Users/sarahsahl/Desktop/relevnt/netlify/functions/ai.ts), [`src/lib/ai/tasks.ts`](/Users/sarahsahl/Desktop/relevnt/src/lib/ai/tasks.ts), [`CoverLetterGenerator.tsx`](/Users/sarahsahl/Desktop/relevnt/src/components/Applications/CoverLetterGenerator.tsx)                                                                                                                     | Tiers, AI providers, resumes, jobs                  |
| Targets and job preferences            | Manage target roles, locations, salary, accessibility/diversity preferences      | Functional   | [`TargetsPage.tsx`](/Users/sarahsahl/Desktop/relevnt/src/pages/TargetsPage.tsx), [`useTargets.ts`](/Users/sarahsahl/Desktop/relevnt/src/hooks/useTargets.ts), [`useJobPreferences.ts`](/Users/sarahsahl/Desktop/relevnt/src/hooks/useJobPreferences.ts)                                                                                                                                                                                                                | Personas, job_preferences                           |
| Auto-apply pipeline                    | Queue, submit, retry, mark submitted/failed, provider-specific apply logic       | Partial      | [`build_auto_apply_queue.ts`](/Users/sarahsahl/Desktop/relevnt/netlify/functions/build_auto_apply_queue.ts), [`auto_apply_mark_submitted.ts`](/Users/sarahsahl/Desktop/relevnt/netlify/functions/auto_apply_mark_submitted.ts), [`auto_apply_mark_failed.ts`](/Users/sarahsahl/Desktop/relevnt/netlify/functions/auto_apply_mark_failed.ts), [`auto_apply/providers/registry.ts`](/Users/sarahsahl/Desktop/relevnt/netlify/functions/auto_apply/providers/registry.ts) | Jobs, personas, tiers, applications                 |
| Admin observability                    | Admin dashboard for users, ingestion, rotation, monitoring, market metrics, logs | Functional   | [`AdminDashboard.tsx`](/Users/sarahsahl/Desktop/relevnt/src/pages/AdminDashboard.tsx), [`admin_ingestion_health.ts`](/Users/sarahsahl/Desktop/relevnt/netlify/functions/admin_ingestion_health.ts), [`admin_logs.ts`](/Users/sarahsahl/Desktop/relevnt/netlify/functions/admin_logs.ts), [`admin_market_metrics.ts`](/Users/sarahsahl/Desktop/relevnt/netlify/functions/admin_market_metrics.ts)                                                                       | Admin RBAC, ingestion tables, snapshots, audit logs |
| Job ingestion engine                   | Scheduled ingestion from APIs/ATS pages with health tracking and healing         | Functional   | [`ingest_jobs.ts`](/Users/sarahsahl/Desktop/relevnt/netlify/functions/ingest_jobs.ts), [`ingestion_healer-background.ts`](/Users/sarahsahl/Desktop/relevnt/netlify/functions/ingestion_healer-background.ts), [`detect-ats.ts`](/Users/sarahsahl/Desktop/relevnt/netlify/functions/detect-ats.ts), [`netlify.toml`](/Users/sarahsahl/Desktop/relevnt/netlify.toml)                                                                                                     | Job sources, jobs, companies                        |
| Public shared audits                   | Public share pages for LinkedIn and portfolio analyses                           | Functional   | [`SharedAuditPage.tsx`](/Users/sarahsahl/Desktop/relevnt/src/pages/SharedAuditPage.tsx), [`linkedin_profiles`](/Users/sarahsahl/Desktop/relevnt/src/types/supabase.ts), [`portfolio_analyses`](/Users/sarahsahl/Desktop/relevnt/src/types/supabase.ts)                                                                                                                                                                                                                 | Supabase public-share tables                        |
| Notifications                          | Realtime user notifications and preference toggles                               | Functional   | [`useNotifications.ts`](/Users/sarahsahl/Desktop/relevnt/src/hooks/useNotifications.ts)                                                                                                                                                                                                                                                                                                                                                                                | Profiles, notifications, Supabase Realtime          |
| Payments/billing                       | Stripe tier config and webhook-driven tier updates                               | Partial      | [`stripe.config.ts`](/Users/sarahsahl/Desktop/relevnt/src/config/stripe.config.ts), [`stripe-webhook.ts`](/Users/sarahsahl/Desktop/relevnt/netlify/functions/stripe-webhook.ts), [`tiers.ts`](/Users/sarahsahl/Desktop/relevnt/src/config/tiers.ts)                                                                                                                                                                                                                    | Profiles, Stripe                                    |

## SECTION 4: DESIGN SYSTEM & BRAND

### 1. Color palette

Verified canonical tokens in [`src/styles/globals.base.css`](/Users/sarahsahl/Desktop/relevnt/src/styles/globals.base.css):

| Name               | Value                       | Defined in                         |
| ------------------ | --------------------------- | ---------------------------------- |
| `--bg`             | `#f8f4ed`                   | `globals.base.css`                 |
| `--surface`        | `#fffcf7`                   | `globals.base.css`                 |
| `--text-primary`   | `#333333`                   | `globals.base.css`                 |
| `--text-secondary` | `#66615e`                   | `globals.base.css`                 |
| `--text-muted`     | `#716c67`                   | `globals.base.css`                 |
| `--border-subtle`  | `#e7e2d9`                   | `globals.base.css`                 |
| `--border-strong`  | `#d0c9bc`                   | `globals.base.css`                 |
| `--accent`         | `#c7a56a`                   | `globals.base.css`                 |
| `--accent-hover`   | `#b8965b`                   | `globals.base.css`                 |
| `--success`        | `#4d7c4d`                   | `globals.base.css`                 |
| `--warning`        | `#b8860b`                   | `globals.base.css`                 |
| `--error`          | `#a65d5d`                   | `globals.base.css`                 |
| `--surface-2`      | `#f0ebe1`                   | legacy alias in `globals.base.css` |
| `--accent-glow`    | `rgba(199, 165, 106, 0.15)` | `globals.base.css`                 |

Verified dark-mode overrides:

- `--bg` `#11100e`
- `--surface` `#1a1917`
- `--text-primary` `#e8e2d7`
- `--text-secondary` `#D1CAC0`
- `--text-muted` `#6a665e`
- `--border-subtle` `#2a2825`
- `--border-strong` `#3d3a35`

Additional theme-provider palette in [`RelevntThemeProvider.tsx`](/Users/sarahsahl/Desktop/relevnt/src/contexts/RelevntThemeProvider.tsx):

- `deepBlack #0D0D0D`
- `softIvory #F5F1E8`
- `surfaceIvory #FDFCF9`
- `champagne #C7A56A`
- `champagneHover #B8965B`
- `graphite #4A4A4A`
- `ink #1A1A1A`

### 2. Typography

Verified fonts:

- `Fraunces` via Google Fonts import
- `Inter` via Google Fonts import
- `JetBrains Mono` tokenized as mono font
- Tailwind config also references `Source Sans 3`, but the active global CSS imports `Inter`, not `Source Sans 3`
- Onboarding CSS contains fallback `'Crimson Text'`, not globally imported

Verified scale:

- `--text-xs 0.75rem`
- `--text-sm 0.875rem`
- `--text-base 1rem`
- `--text-lg 1.125rem`
- `--text-xl 1.25rem`
- `--text-2xl 1.5rem`
- `--text-3xl 2rem`
- `--text-4xl 2.5rem`
- extended: `--text-nano`, `--text-input`, `--text-body-strong`, `--text-micro`, `--text-tiny`
- locked semantic ramp for page title, section, label, body, meta in `globals.base.css`

### 3. Component library

Verified reusable UI components under [`src/components/ui`](/Users/sarahsahl/Desktop/relevnt/src/components/ui), [`src/components/shared`](/Users/sarahsahl/Desktop/relevnt/src/components/shared), [`src/components/forms`](/Users/sarahsahl/Desktop/relevnt/src/components/forms), and [`src/components/canonical`](/Users/sarahsahl/Desktop/relevnt/src/components/canonical):

```text
Alert: inline alert/status messaging
AutosaveIndicator: save-state feedback
Badge: pill label
Button: primary/secondary/ghost actions
Card: generic content container
Chip: legacy pill component
CollectionEmptyGuard: empty-state guard for collections
ConfirmDialog: destructive/confirmation modal
CustomIcon / CustomIcons: branded icon rendering
EmptyState / CanonicalEmptyState: reusable empty states
ErrorBoundary: React error boundary
FeatureIcon: feature/marketing icon wrapper
FieldBlock: grouped field layout
FiltersBar / InlineFilterBar: filter UI bars
FormSection: structured form section wrapper
Icon: application icon wrapper
Input: text input primitive
IntentSummary: concise summary block
JobListItem: canonical job row
ListItem: generic list row
LoadingState: loading UI
NetworkStatusBanner: online/offline banner
PageHeader / CanonicalPageHeader: page heading patterns
PageHero: route hero header
PageProfileProvider: page-style/profile scoping
PreferenceRow: settings/preference row
PrimaryActionRegistry: action coordination wrapper
ProfileErrorBanner: profile-load failure banner
QuietControls: subdued control cluster
RangeControl: slider/range input
ResumeEditConflictBanner: multi-tab edit conflict warning
RowActions: row action wrapper
ScoreLabel: score badge/label
Section / SectionNav: content sectioning
SegmentedControl: segmented switcher
SettingsItem: settings row wrapper
Stack: spacing/layout primitive
Tabs: tabbed interface
Toast: toast notifications
Typo / Typography: text primitives
WelcomeModal: onboarding/welcome modal

Checkbox / Select / Textarea: form primitives
Container / PageBackground / PageHeader / Modal / Spinner / UsageStats / JobFeed: shared layout and utility components
```

### 4. Design language

Verified style language from code/comments/tokens:

- editorial / ledger / journal metaphor
- warm ivory surfaces, champagne-gold accent
- restrained borders, minimal rounding, minimal shadows
- “ink on ivory paper” / “champagne gold journal” is explicitly stated in CSS comments
- not playful or consumer-bright; more mature and editorial

### 5. Responsive strategy

Verified patterns:

- React pages use responsive flex/grid utility classes and CSS media queries
- route-level pages use mobile-first stacked layouts with desktop enhancements
- `JobsPage` and marketing sections rely on `auto-fit` grids and breakpoint classes
- CSS media query found for marginalia collapse at `max-width: 1200px`
- no dedicated separate mobile app/web split found

### 6. Dark mode

Verified support:

- `darkMode: 'class'` in [`tailwind.config.ts`](/Users/sarahsahl/Desktop/relevnt/tailwind.config.ts)
- CSS variable dark overrides in `globals.base.css`
- `RelevntThemeProvider` persists `Light`, `Dark`, and `DarkAcademia` modes to localStorage and sets classes/data attributes

Gap:

- `DarkAcademia` exists in theme provider types/state, but I did not find a matching full token set in the inspected global CSS.

### 7. Brand assets

Verified assets:

- `public/relevnt_final/assets/logos/relevnt_glyph.*`
- `public/relevnt_final/assets/logos/relevnt_wordmark.*`
- `public/relevnt_final/assets/logos/relevnt_horizontal_lockup.*`
- `public/relevnt_final/assets/logos/relevnt_vertical_lockup.*`
- `public/relevnt_final/assets/images/hero_bg.png`
- `public/relevnt_final/assets/resume/*.png` illustrations for resume sections
- `public/icon-sprite.svg`, `sprite-dark-cloudinary.svg`, `sprite-light-cloudinary.svg`

## SECTION 5: DATA & SCALE SIGNALS

### 1. User model

Verified user-related data in `profiles`:

- identity: `id`, `email`, `full_name`, `preferred_name`, `avatar_url`, `location`, `timezone`
- product state: `tier`, `plan_tier`, `edu_verified`, `onboarding_*`, `auto_apply_active`
- preferences: notification flags, theme, layout density, data-sharing toggle
- career/voice: narrative fields, priorities, voice controls
- admin flags: `is_admin`, `admin_level`

Verified journey from code:

1. Signup/login via Supabase
2. `create_profile` bootstrap
3. Optional `.edu` verification upgrades to Pro
4. onboarding wizard creates first target
5. user can upload resume or skip
6. browse/match/save jobs
7. track applications and generate AI artifacts
8. optionally use admin/market/auto-apply features depending on role/tier

### 2. Content/data volume

Verified scale signals:

- ingestion schedules explicitly target “100K+” job volume in `netlify.toml` comments
- pagination everywhere:
  - jobs page `PAGE_SIZE = 50`
  - applications page `PAGE_SIZE = 50`
  - admin APIs support `limit/offset`
- ingestion tables and history/archive tables imply large-volume operational intent
- no committed seed dataset for end-user content found; there are SQL validation/seed helper scripts for auto-apply/ops

### 3. Performance considerations

Verified:

- route-level lazy loading in [`App.tsx`](/Users/sarahsahl/Desktop/relevnt/src/App.tsx)
- React Query `staleTime`/`gcTime`, retry policy, no refetch-on-focus
- pagination in jobs/applications/admin logs
- debounce usage in jobs and autosave
- cache tables and cache invalidation logs
- client-side dedup for job lists
- realtime subscriptions for collections/notifications/jobs
- Vite chunk warning limit set; comment says lazy-loaded pages keep chunks under threshold

### 4. Analytics/tracking

Verified analytics events in [`analytics.service.ts`](/Users/sarahsahl/Desktop/relevnt/src/services/analytics.service.ts):

- signup/login/logout/password reset
- onboarding start/completion
- analysis run/limit reached
- resume optimized
- interview prep started
- skill gap viewed
- job searched/saved/applied
- application tracked
- persona switched
- auto-apply opened/submitted/failed
- upgrade prompt/click/tier changes/subscription events
- promo/reactivation
- error/api failure

Gap:

- this service posts to `/.netlify/functions/analytics`, but the actual committed analytics endpoint is `track_event.ts`. I did not find a matching `analytics.ts` Netlify Function.

### 5. Error handling

Verified patterns:

- frontend `ErrorBoundary`
- centralized client error reporting helper with future Sentry hook point
- server-side `log_operation_failure` path to `failure_logs`
- audit logging to `audit_logs`
- rate-limit and validation errors return structured JSON
- many hooks catch and surface user-readable errors

Gap:

- Sentry integration is not wired; code contains TODO placeholder only.

### 6. Testing

Verified test file count: `71`.

Coverage areas from file names:

- Netlify AI router, caps, cache, routing
- personas/tuner settings
- ingestion and RSS parsing
- auto-apply queue/provider rules
- auth/JWT verification
- application status transitions and duplicate handling
- concurrent edits/offer acceptance/application integrity
- data integrity and logging audits
- resume upload/default/versioning
- hooks for targets/resumes/matched jobs
- scoring engine, keyword extraction, profile aggregation
- matching service and profile parser
- SQL integrity test suite in [`tests/data-integrity-test-suite.sql`](/Users/sarahsahl/Desktop/relevnt/tests/data-integrity-test-suite.sql)

Not done in this audit:

- I did not execute the test suite.

## SECTION 6: MONETIZATION & BUSINESS LOGIC

### 1. Pricing/tier structure

Verified in [`src/config/tiers.ts`](/Users/sarahsahl/Desktop/relevnt/src/config/tiers.ts):

- `Starter`: free
- `Pro`: `$19/mo`
- `Premium`: `$49/mo`

Verified in [`stripe.config.ts`](/Users/sarahsahl/Desktop/relevnt/src/config/stripe.config.ts):

- `pro_monthly` `$19`
- `pro_annual` `$180`
- `premium_monthly` `$49`
- `premium_annual` `$399`
- `enterprise` custom/contact
- add-ons: extra analysis, interview coaching, export pack
- coupons: EDU promo, early adopter

### 2. Payment integration

Verified:

- Stripe webhook exists and updates user tier/status from webhook events
- no Stripe SDK dependency was found; signature verification is manual HMAC logic
- no committed checkout/session creation frontend/backend flow was found in inspected files

Assessment: `Partial`

### 3. Subscription/billing logic

Verified:

- webhook handles:
  - `customer.subscription.created`
  - `customer.subscription.updated`
  - `customer.subscription.deleted`
  - `invoice.payment_failed`
- profile fields updated with `stripe_subscription_id`, `stripe_subscription_status`
- tier downgrade on subscription deletion
- annual pricing config exists

Not found:

- billing portal flow
- checkout session creation
- invoice UI
- trial-period implementation beyond Stripe comments/status handling

### 4. Feature gates

Verified gates from `tiers.ts` / `FeatureGate.tsx` / server tier guards:

- Starter:
  - job search
  - resume upload
  - resume extract
- Pro adds:
  - resume analyze
  - resume optimize
  - cover-letter generate
  - interview prep
  - skill-gap analysis
- Premium adds:
  - analytics dashboard
  - AI coaching

### 5. Usage limits

Verified limits in `tiers.ts`:

- Starter:
  - analyses/month `5`
  - resumes `2`
  - saved jobs `50`
  - applications `10`
  - auto-apply/week `5`
- Pro:
  - analyses/month `50`
  - resumes `10`
  - cover letters `10`
  - interview sessions `20`
  - saved jobs `200`
  - applications `50`
  - auto-apply/week `50`
- Premium:
  - effectively unlimited/high caps
- AI daily/high-quality caps also defined
- server-side `increment_usage_if_under_limit` and tier guards exist

## SECTION 7: CODE QUALITY & MATURITY SIGNALS

### 1. Code organization

Verified positives:

- clear `pages` / `hooks` / `components` / `lib` / `services` / `netlify/functions` split
- generated DB types
- central tier module
- centralized query invalidation and analytics/error helpers

Verified negatives:

- historical/archive folders are large
- deprecated endpoints remain in place
- some direct browser-to-Supabase CRUD bypasses server-layer consistency

### 2. Patterns and conventions

Verified patterns:

- hook-heavy React architecture
- React Query for async state
- context providers for auth/theme
- server utilities for auth, CSRF, rate limiting, logging
- DB-side enforcement via triggers/RPCs/RLS
- naming is mostly consistent, but there is mixed legacy vocabulary: `persona`, `target`, `tuner`, `canonical`, `ready`

### 3. Documentation

Verified:

- good inline code comments in many core files
- `docs/ui/README.md`
- audit docs and scripts
- no root README

Assessment: internal engineering documentation is better than external product/repo documentation.

### 4. TypeScript usage

Verified:

- TypeScript is used broadly on frontend and most functions
- generated `Database` types from Supabase
- many explicit interfaces/types
- still some `any` usage and casts, especially around Supabase rows and legacy boundaries
- several files use `(supabase as any)` or `Record<string, any>`

Assessment: strong but not strict-clean.

### 5. Error handling patterns

Verified:

- consistent `try/catch` in functions and hooks
- structured error logging utilities
- user-facing error translation in auth/application flows
- no custom frontend error-class hierarchy found in inspected files

### 6. Git hygiene

Verified recent patterns:

- PR merge commits
- Dependabot updates
- `feat:` conventional messages
- audit/plan commits such as `Initial plan`
- active audit/triage workflow in commit history

Assessment: active repo hygiene, but recent history is heavily audit/debt/automation oriented.

### 7. Technical debt flags

Verified flags:

- missing root README
- deprecated routes/endpoints moved to Ready still present
- `analytics.service.ts` points to non-existent endpoint
- `learning-search` edge function references tables not present in current typed schema
- Tailwind config exists without `tailwindcss` dependency
- TODOs in error reporting and tests
- large migration archive/backups in repo
- audit artifacts indicate repeated schema/type cleanup work; recent commits explicitly fix audit findings

### 8. Security posture

Verified positives:

- Supabase RLS broadly enabled
- backend admin verification separate from frontend admin UI checks
- CSRF tokens for sensitive writes
- rate limiting utility with fail-open/fail-closed modes
- JWT verification helpers
- analytics sanitizes obvious PII fields
- search filter validation exists
- Stripe webhook verifies signature and rejects stale payloads

Verified concerns:

- some deprecated endpoints still use wildcard CORS in stub responses
- many client-side direct Supabase mutations mean security depends heavily on RLS correctness
- secrets are env-based; no `.env` values committed, which is good
- no fully wired external error-monitoring/security-alerting service found

## SECTION 8: ECOSYSTEM CONNECTIONS

### 1. Shared code or patterns with other Penny Lane Project apps

Verified:

- cross-product config explicitly links to Ready in [`src/config/cross-product.ts`](/Users/sarahsahl/Desktop/relevnt/src/config/cross-product.ts)
- multiple Netlify Functions return “This endpoint has moved to Ready and will be removed.”

### 2. Shared dependencies or infrastructure

Verified:

- same GitHub org: `thepennylaneproject`
- same general stack pattern: Netlify + Supabase is strongly implied for portfolio consistency, but only Relevnt is directly inspectable here
- no shared package import from sister repos was found

### 3. Data connections

Verified:

- no direct code import or DB connection string proving shared database across sister projects
- `Ready` is referenced as a destination for moved functionality, but no shared DB identity is committed here

### 4. Cross-references

Verified cross-project references:

- Ready URL: `https://relevnt.work/ready`
- deprecated function messages: “moved to Ready”
- no verified code references to `Codra`, `Mythos`, `embr`, `passagr`, or `advocera`

## SECTION 9: WHAT'S MISSING (CRITICAL)

### 1. Gaps for a production-ready product

1. Verified billing is incomplete. The repo has pricing config and a webhook, but I did not find checkout/session creation, billing portal, or a committed purchase flow.
2. Analytics wiring is inconsistent. `analytics.service.ts` posts to a missing `/.netlify/functions/analytics` route while `track_event.ts` is the real endpoint.
3. The schema surface is not fully coherent. `learning-search` references tables not present in the current typed schema.
4. Several endpoints are still legacy stubs that return “moved to Ready,” which weakens the repo’s boundary clarity.
5. Root-level product/repo documentation is missing, including setup, deployment, and operator guidance.

### 2. Gaps for investor readiness

1. No verified main production URL or deployment target is committed.
2. No committed dashboards, KPI exports, or revenue instrumentation beyond admin tables/config and event schemas.
3. No committed architecture overview for investors/partners.
4. No clear live customer evidence in repo config.
5. Payment/commercial flow is not end-to-end verifiable from code.

### 3. Gaps in the codebase itself

1. Dead/moved endpoint files remain in `netlify/functions`.
2. `analytics.service.ts` likely points at dead infrastructure.
3. Theme system contains a `DarkAcademia` mode without matching full inspected CSS token implementation.
4. Tailwind config appears partially vestigial without `tailwindcss` package.
5. Large migration archives/backups make current-state reasoning harder.

### 4. Recommended next steps

1. Finish the billing path end-to-end: add checkout/session creation, billing portal, and validated Stripe IDs. This closes the clearest monetization gap.
2. Normalize API boundaries: remove or isolate Ready-moved stubs, and fix all callers to the live endpoint set. This reduces architectural ambiguity.
3. Reconcile schema drift: either add missing learning tables or remove the edge function; regenerate types from one canonical migration chain. This improves trust in the data layer.
4. Fix analytics plumbing: point all frontend tracking to `track_event` or add the missing `analytics` function. This unlocks trustworthy growth metrics.
5. Publish a root README plus deployment/runbook. This is low effort and materially improves diligence readiness.

## SECTION 10: EXECUTIVE SUMMARY

Relevnt is an AI-assisted career intelligence web application for job seekers. The verified product surface covers signup/login, onboarding, resume upload and editing, job discovery and matching, saved jobs, application tracking, AI-assisted resume and cover-letter workflows, notifications, and an admin-side ingestion/market intelligence console. The positioning in the code is clear: help users evaluate whether to apply, tailor their story, and navigate the job market with more signal and less guesswork.

Technically, the repo shows real build depth. It is a React/Vite SPA backed by Netlify Functions and a large Supabase/Postgres schema with generated types, RLS, background ingestion, rotation/healing logic, analytics tables, audit/failure logging, AI routing across multiple providers, and tier-based usage controls. The builder has implemented not just UI, but operational systems: job ingestion pipelines, admin observability, CSRF/rate limiting, database invariants, and structured logging. That signals strong full-stack capability, especially around data-heavy workflow software.

The current state is credible but not yet investor-clean. Core product mechanics are present, but billing is only partially verifiable, several endpoints are explicitly being moved to Ready, analytics wiring has at least one visible mismatch, and the repo lacks a root README and a committed canonical production URL. In practical terms, the next milestone is not “invent the product,” but “tighten the surface”: finalize monetization, simplify boundaries, reconcile schema drift, and document the live system clearly enough that a third party can trust and operate it.

```text
---
AUDIT METADATA
Project: relevnt-frontend
Date: 2026-03-07
Agent: GPT-5 Codex
Codebase access: full repo
Confidence level: medium — high confidence on manifests, routes, schema, CI, tiers, and major features from primary files; lower confidence on a few edge/scheduled endpoints and full runtime completeness because this was a static audit and tests were not executed.
Sections with gaps: 1, 2, 5, 8, 9
Total files analyzed: 52
---
```
