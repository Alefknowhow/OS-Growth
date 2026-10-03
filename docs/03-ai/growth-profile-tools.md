# Growth Profile Agent Tools

Purpose-specific tools prevent agents from receiving an uncontrolled dump of all client context.

## get_client
Returns identity, status and compact business summary.

## get_growth_profile
Returns a scoped profile view selected by sections and workflow purpose.

Suggested input:
- client_id
- sections[]
- as_of nullable

## get_offers
Filters active/prioritized offers and commercial constraints.

## get_personas
Returns ICP/persona context relevant to an offer or campaign.

## get_goals
Returns active goals, KPI targets and measurement period.

## get_budget_context
Returns active budget allocations and operating constraints.

## get_funnel
Returns mapped stages, expected rates and CRM/revenue mappings.

## get_creative_context
Returns brand/creative rules and approved learnings.

## search_client_knowledge
Retrieves evidence from the client Knowledge Base with source references.

## get_client_decisions
Returns relevant strategic decisions effective for a requested date/context.

## propose_client_fact
Allows AI to propose a new fact as inferred/needs_review. It cannot silently create a confirmed fact.

## confirm_client_fact
Human-authorized workflow/tool for promoting reviewed facts to confirmed status.

## Context packs
The application may expose deterministic context-pack builders:
- performance_analysis
- creative_strategy
- content_generation
- reporting
- experiment_review

Each pack defines required/optional sections and token/data limits. This makes agent behavior reproducible and observable.
