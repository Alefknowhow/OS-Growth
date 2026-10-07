# Growth Profile Agent Tools

Purpose-specific tools prevent agents from receiving an uncontrolled dump of all client context. Canonical names and classes are in `tools.md`; this file details the profile subset.

## get_client
Identity, status, readiness and compact business summary.

## get_growth_profile
Scoped profile view selected by sections and workflow purpose.
Input: `client_id`, `sections[]`. Historical reproduction uses `context_snapshots`, not an `as_of` parameter.

## get_offers / get_personas / get_goals / get_budget_context / get_funnel / get_creative_context / get_brand_kit
Section reads. `get_creative_context` merges manual seeds with validated learnings (winning/losing angles, hooks).

## search_client_knowledge
Evidence from the Knowledge Base with source references.

## get_client_decisions
Strategic decisions effective for a date/context.

## propose_client_fact
Proposes a fact as `inferred`/`needs_review` with provenance. Cannot create confirmed facts.

## confirm_client_fact (human-only)
Promotes a reviewed fact to `confirmed` and applies it to the mapped profile field (see `../02-architecture/growth-profile-data-model.md`).

## Context packs
| Pack | Required | Optional |
|---|---|---|
| performance_analysis | goals, KPIs, conversion definitions, budget, signals, metrics | funnel, decisions, learnings |
| media_buying | goals, budget, envelope summary, insights, action history, entity state | learnings, seasonality |
| creative_strategy | offers, personas, positioning, creative performance, learnings | competitors, knowledge |
| content_generation | brief, offer, persona, tone, approved/prohibited claims | examples of winning copy |
| creative_production | brief, skill spec, brand kit, candidate assets with consent | previous variants |
| reporting | goals, period metrics, executed actions, outcomes, learnings | client notes |
| command_resolution | client list (assigned), entity names index | recent commands |

Each pack defines token/data limits and freshness requirements; runs fail visibly if required sections are missing (and readiness reflects it).
