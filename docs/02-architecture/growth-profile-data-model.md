# Growth Profile Data Model

This document translates the Client / Growth Profile into an initial relational model. Final SQL migrations should be produced during implementation.

## Core ownership

### clients
Identity of the operational client inside Growth OS.

Suggested fields:
- id uuid
- organization_id uuid
- name text
- slug text
- status enum
- website_url text nullable
- industry text nullable
- timezone text nullable
- crm_mapping_id uuid nullable
- created_at
- updated_at

### client_profiles
Current high-level growth context.

Suggested fields:
- id uuid
- organization_id uuid
- client_id uuid unique
- business_summary text
- business_model text
- service_area jsonb
- sales_model text
- average_sales_cycle_days integer nullable
- seasonality jsonb
- positioning_statement text
- value_proposition text
- tone_of_voice jsonb
- approved_claims jsonb
- prohibited_claims jsonb
- operational_constraints jsonb
- profile_completeness numeric
- last_reviewed_at timestamptz nullable
- created_at
- updated_at

Do not put all domain collections into this row. Offers, personas, goals, competitors and knowledge are separate entities.

## Domain collections

### client_offers
id, organization_id, client_id, name, description, category, price_min, price_max, currency, margin_percent nullable, priority, landing_page_url, capacity_constraints, differentiators, conditions, status, created_at, updated_at.

### client_personas
id, organization_id, client_id, name, segment, attributes jsonb, pains jsonb, desires jsonb, jobs_to_be_done jsonb, objections jsonb, triggers jsonb, disqualifiers jsonb, decision_criteria jsonb, awareness_stage, status.

### client_goals
id, organization_id, client_id, name, objective_type, start_date, end_date, primary_kpi, target_value, secondary_kpis jsonb, attribution_model nullable, constraints jsonb, status.

### client_budgets
id, organization_id, client_id, period_start, period_end, total_amount, currency, channel_allocations jsonb, test_budget nullable, rules jsonb.

### client_funnel_stages
id, organization_id, client_id, name, position, source_system, source_stage_id nullable, expected_conversion_rate nullable, revenue_stage boolean.

### client_competitors
id, organization_id, client_id, name, url, positioning, offer_summary, observations jsonb, evidence jsonb, last_reviewed_at.

### client_creative_context
id, organization_id, client_id, pillars jsonb, approved_formats jsonb, visual_constraints jsonb, winning_angles jsonb, losing_angles jsonb, proof_assets jsonb, production_constraints jsonb.

### knowledge_items
id, organization_id, client_id, title, type, source_url nullable, storage_path nullable, extracted_text/reference, status, metadata jsonb, created_at, updated_at.

### client_decisions
id, organization_id, client_id, category, decision, rationale, effective_at, status, source_type, source_reference, created_by, created_at.

### integration_mappings
id, organization_id, client_id, provider, external_account_id, external_entity_type, external_entity_id, metadata jsonb, status.

## Fact provenance

For fields that require independent review/history, prefer a fact record rather than endlessly expanding columns.

### client_facts
- id
- organization_id
- client_id
- namespace
- key
- value jsonb
- status: confirmed | inferred | outdated | needs_review
- source_type
- source_reference
- confidence nullable
- captured_by
- captured_at
- reviewed_by nullable
- reviewed_at nullable
- supersedes_fact_id nullable

This is useful for AI-derived or CRM-derived facts without making the primary relational model unstructured.

## Security
- RLS on tenant-owned tables.
- organization_id must be derived/validated server-side, not trusted from arbitrary client input.
- Client portal access is separately constrained by client membership/mapping.
- Integration credentials live in secure connection storage, never profile tables.

## Index direction
At minimum:
- organization_id
- organization_id + client_id
- provider + external IDs
- status/date indexes for goals, budgets and decisions
- vector/search indexes only when Knowledge Base retrieval is implemented.

## Versioning
Historical campaign/experiment decisions must retain the context they were made against. Prefer snapshots/references for consequential AI runs rather than assuming the current profile represents historical truth.
