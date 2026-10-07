# Performance Data Model

How ad-platform data flows from raw API responses to normalized metrics agents reason over. Meta is the reference implementation; other providers map into the same tables.

## Layers
1. **Raw** — `raw_ingestion_batches`: exact provider responses (or storage path for large payloads), request params, window, fetched_at, checksum, sync_run_id. Never edited.
2. **Entities** — current state of accounts, campaigns, ad sets, ads, creatives, plus change history.
3. **Facts** — daily (and intraday) metrics at the lowest grain, conversions by definition.
4. **Views** — derived metrics and rollups for UI, signals and agents.

## Entities
### ad_accounts
id, organization_id, client_id, provider, external_id, name, currency, timezone, status, connection_id, business_id, last_synced_at.
Invariant: one ad account belongs to exactly one client.

### campaigns / ad_sets / ads
Common: id, organization_id, client_id, ad_account_id, provider, external_id, parent ids, name, status (configured), effective_status (delivered), created_time, updated_time, last_seen_at, raw_ref.
- campaigns: objective, buying_type, budget_mode (CBO/ABO), daily_budget, lifetime_budget, bid_strategy, special_ad_categories.
- ad_sets: optimization_goal, billing_event, daily_budget, lifetime_budget, bid_amount, targeting (jsonb snapshot), placements, schedule, learning_stage.
- ads: ad_creative_id, creative_id (internal, nullable), tracking specs.

### ad_creatives
Provider creative: external_id, type, body, title, link, cta, media refs, thumbnail_url (copied to storage). Linked to internal `creatives` through `creative_publications`.

### entity_changes
Every detected change: entity ref, field, old, new, changed_at, source (`growth_os` with action_execution_id | `external` from provider activity log | `sync_diff`). Used for change history, drift detection and attributing outcomes.

## Metrics
### metric_daily (grain: ad × date)
organization_id, client_id, ad_account_id, campaign_id, ad_set_id, ad_id, date, currency, spend, impressions, clicks, link_clicks, landing_page_views, video_plays_3s, video_thruplays, video_p25…p100, post_engagements, sync_version, synced_at.
Unique `(ad_id, date)`; upserted idempotently.

Reach and frequency are **not additive**: fetched separately at campaign/ad set/account level per period into `metric_reach_period` when needed.

### metric_intraday
Today's spend/results per ad set, refreshed frequently (e.g. hourly) for pacing and guardrails. Replaced by daily data once the day closes.

### conversion_definitions (per client)
name ("lead", "purchase", "qualified_lead"), provider, source_action_type(s) (e.g. pixel/dataset event, lead form), attribution_setting (e.g. 7d click / 1d view), value_field, is_primary, active.
The client's goals reference a conversion definition, so "CPL" always means a specific, versioned definition.

### metric_conversions_daily
ad_id, date, conversion_definition_id, attribution_setting, count, value, currency.

### fx_rates
For portfolio views in the agency currency; client reports use the ad account currency.

## Views (examples)
`v_ad_daily` (CTR, CPC, CPM, CPA, ROAS, hook rate), `v_adset_daily`, `v_campaign_daily`, `v_client_daily`, `v_client_pacing` (spend MTD vs budget plan), `v_creative_performance`.

## Sync strategy (Meta)
- **Entities:** full refresh daily + incremental by `updated_time` every few hours; diffs written to `entity_changes`.
- **Insights daily:** ad-level, `time_increment=1`, rolling restatement window (default last 28 days, configurable) because attributed conversions update after the fact.
- **Intraday:** current day ad-set level, hourly.
- **Activity log:** pull account activities to attribute external edits.
- **Backfill:** on connection, up to 13 months (configurable) in chunked jobs.
- Locking per ad account (Redis) and provider rate-limit budgeting; failures recorded in `sync_runs` with error class.
- `meta.sync.completed` event triggers signal detection.

## Data quality checks
Spend reconciliation (sum of ads vs account total), missing days, sudden zero conversions with normal clicks (tracking break), currency/timezone changes. Failures create `signals` of type `data_quality` and block automatic execution for that client until resolved.
