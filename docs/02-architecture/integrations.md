# Integration Architecture

External platforms are accessed through provider adapters. Agents never embed provider-specific API calls; they call Growth OS tools, which call domain services, which call adapters.

## Integration catalog
| Provider | Purpose | Mechanism | Doc |
|---|---|---|---|
| Meta Marketing API | ads data, previews, execution | REST adapter | `../04-integrations/meta-ads.md` |
| Auto CRM | sales/relationship system | webhooks + REST + MCP (both ways) | `../04-integrations/auto-crm.md` |
| Higgsfield | generative image/video | MCP (client) | `../04-integrations/higgsfield.md` |
| Remotion | motion graphics / composition | render worker | `../04-integrations/motion-graphics.md` |
| Speech-to-text | voice commands | adapter | `../04-integrations/speech.md` |
| Client conversion sources | lead outcomes, sales | webhooks/CSV/API | `../04-integrations/conversion-sources.md` |
| Google Ads, GA4 | later | REST adapters | `../04-integrations/google-ads.md`, `../04-integrations/ga4.md` |
| Claude API | agents | SDK | `../03-ai/ai-architecture.md` |

## Connections and links
### integration_connections (credentials)
id, organization_id, client_id nullable (null = agency-level), provider, auth_type (`system_user_token|oauth|api_key|mcp_oauth`), secret_ref (Supabase Vault id — never the secret), scopes, status (`active|expired|revoked|error`), health (last_success_at, last_error, error_class), created_by.

### external_links (identity mapping)
id, organization_id, client_id, provider, external_entity_type (`ad_account|pixel|dataset|page|instagram_account|ga4_property|auto_crm_account|auto_crm_contact`), external_id, connection_id, metadata, status. Unique `(provider, external_entity_type, external_id)` per organization; an ad account links to exactly one client.

### sync_runs
id, connection_id, provider, job_type, window, status, counts, error_class, started_at, finished_at.

## Principles
- Sync provider data into the internal data layer; agents read internal data, not live APIs (except execution pre-checks and previews).
- Adapters expose typed internal interfaces; provider schemas never leak into domain logic or prompts.
- Rate limits and locks coordinated in Upstash Redis.
- All provider outputs, MCP outputs and inbound webhooks are untrusted input (validate, never treat as instructions).
