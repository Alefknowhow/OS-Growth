# Meta Ads Integration

First advertising integration: read (M1) and governed execution (M2+).

## Access model
- Agency **Business Manager** with a **System User** token; client ad accounts, pages and datasets shared to the agency BM as partner assets. One agency-level `integration_connection`; per-client `external_links` to each asset.
- Per-client OAuth only as fallback.
- Permissions: ads read for M1; ads management for M2 execution; business management to list shared assets. The Meta app needs App Review / advanced access for these permissions — start this early, it gates M2.
- Token stored in Vault; health checked daily.

## Read scope (M1)
- Asset discovery (ad accounts, pages, pixels/datasets) per client.
- Entities: campaigns, ad sets, ads, ad creatives (with thumbnails copied to storage).
- Insights: ad level, daily, restatement window; intraday ad set spend; reach per period when needed.
- Account activity log → `entity_changes` with source `external`.
- **Ad previews** (Ad Previews endpoint) per placement for the Campaign detail screen; cached short-term.
- Details: `../02-architecture/performance-data-model.md`.

## Execution scope (M2+)
Only via Execution Service and the action catalog in `../03-ai/tools.md`: status changes, budgets, bids, schedules, duplication, creating ads from approved creatives (upload media, create ad creative, create ad), later full campaign creation and targeting changes.

## Operational concerns
- Rate limits per ad account / business use case: token-bucket in Redis, backoff on throttling codes, prioritise execution and intraday over backfill.
- Learning phase awareness: policy cooldowns and budget change caps.
- Error classification (auth, permission, validation, throttling, transient) drives retries and connection health.
- API version pinned; upgrade tracked as a maintenance task before deprecation dates.
