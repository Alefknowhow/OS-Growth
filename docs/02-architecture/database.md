# Database Direction

PostgreSQL/Supabase is the primary system of record for Growth OS.

## Core entities
organizations, users, memberships, clients, client_profiles, goals, knowledge_items, integration_connections, ad_accounts, campaigns, ad_sets, ads, creative_assets, metric_daily, conversions, projects, tasks, experiments, experiment_results, reports, ai_runs, ai_actions, approvals, audit_logs, memories.

## Rules
- Tenant data carries organization_id.
- Client operational data carries client_id where applicable.
- External IDs are namespaced by provider/account.
- Store normalized metrics separately from raw ingestion payloads.
- Preserve provenance for AI inputs and decisions.
- Do not copy full CRM datasets; store CRM references and purpose-specific projections.
