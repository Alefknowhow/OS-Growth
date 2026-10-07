# Database Direction

PostgreSQL (Supabase) is the system of record for Growth OS.

## Entity map

| Domain | Tables | Spec |
|---|---|---|
| Tenancy & access | organizations, users, memberships, client_assignments, client_portal_users | `../01-product/permissions.md` |
| Clients & profile | clients, client_profiles, client_offers, client_personas, client_goals, client_budgets, client_funnel_stages, client_competitors, client_creative_context, client_decisions, client_facts, knowledge_items | `growth-profile-data-model.md` |
| Integrations | integration_connections, external_links, sync_runs, raw_ingestion_batches, inbound_webhooks, outbox_events | `integrations.md`, `event-architecture.md` |
| Paid media | ad_accounts, campaigns, ad_sets, ads, ad_creatives, entity_changes, metric_daily, metric_intraday, conversion_definitions, metric_conversions_daily, fx_rates | `performance-data-model.md` |
| Operations / AI loop | signals, insights, hypotheses, plans, plan_actions, approvals, action_executions, action_outcomes, autopilot_envelopes, policy_rules, commands, recommendation_feedback | `operations-data-model.md` |
| Creative | media_assets, media_asset_derivatives, asset_consents, brand_kits, creative_skills, creative_briefs, creative_jobs, creative_job_steps, creatives, creative_reviews, creative_publications | `creative-data-model.md` |
| Learning | experiments, experiment_variants, experiment_results, learnings, memories | `operations-data-model.md`, `../03-ai/memory.md` |
| Work & reporting | projects, tasks, reports, report_schedules | `operations-data-model.md` |
| AI observability | ai_runs, ai_tool_calls, context_snapshots, prompt_versions, eval_runs | `operations-data-model.md` |
| Audit | audit_logs (append-only) | `security.md` |

## Rules
- Every tenant table has `organization_id`; client-scoped tables also have `client_id`.
- Client-scoped tables use a composite FK `(client_id, organization_id) → clients(id, organization_id)` so the two can never diverge.
- RLS on all tenant tables; policies based on membership + client assignment (members) or portal binding (clients).
- External IDs are namespaced: unique `(provider, external_account_id, external_id)`.
- Raw ingestion payloads are stored separately from normalized data and retained per policy.
- Store additive base metrics only; ratios (CTR, CPA, ROAS) are computed in views.
- Money: `numeric(14,2)` plus explicit `currency`; never floats.
- Timestamps `timestamptz`; metric dates are `date` in the ad account timezone.
- `audit_logs` and `action_executions` are append-only (no UPDATE/DELETE grants).
- Do not copy Auto CRM datasets; store links and purpose-specific projections only.
- Migrations are versioned and reviewed; generated TypeScript types from the schema.
