# Auto CRM Integration

## Boundary
| Auto CRM (sales platform) | Growth OS (delivery platform) |
|---|---|
| Leads/prospects of the agency, conversations, meetings, proposals, contracts, billing, relationship, account owner tasks | Ad accounts, media, creatives, plans, executions, performance, reports, learnings |
| "Who is the client and what did we sell?" | "How is the delivery going and what are we doing about it?" |

Neither system copies the other's datasets. They share identifiers, events and purpose-specific projections.

> Note: Auto CRM manages the **agency's** relationship with its clients. The **client's own** leads and sales (used to compute true CPA/ROAS) come from client conversion sources — see `conversion-sources.md`. If Auto CRM is later also used to manage client leads, it becomes one of those sources through the same contract.

## How the connection works (three layers)

### 1. Identity link
`external_links` row: `client_id ↔ auto_crm_account_id` (plus optional primary contact ids). Created automatically when Growth OS handles `crm.deal.won`; can be linked manually for existing clients.

### 2. Events / webhooks (state sync — reliable, async)
**Auto CRM → Growth OS** (signed webhooks to `/api/webhooks/auto-crm`):
| Event | Growth OS reaction |
|---|---|
| `crm.deal.won` | create client, prefill profile facts (company, segment, contracted services, media budget, fee scope), start onboarding, link identity |
| `crm.contract.updated` | update scope/budget facts (`needs_review`), adjust envelope suggestions |
| `crm.meeting.processed` | store summary reference; Onboarding/Context agent proposes facts (goals, offers, objections) for review |
| `crm.contact.updated` | update portal user invitations if relevant |
| `crm.client.churned` | freeze automation (kill switch), final report, offboarding/retention workflow |

**Growth OS → Auto CRM** (outbox → signed webhooks):
| Event | Purpose in Auto CRM |
|---|---|
| `growth.client.onboarded` | mark delivery started |
| `growth.report.published` | log activity, notify account owner/client |
| `growth.client.health.changed` / `growth.client.risk_detected` | create relationship task for the account owner (e.g. call the client) |
| `growth.approval.needed_from_client` | optional: let Auto CRM notify the client through its channels |

### 3. MCP (agent access — on demand)
- **Growth OS agents → Auto CRM MCP:** read relationship context when reasoning (contract scope, client sensitivities, last meeting summary, upcoming renewal) via wrapped tools such as `get_crm_relationship_context`. Writes limited to logging activities and creating tasks, always through Growth OS policy and audit.
- **Auto CRM's AI → Growth OS MCP server:** scoped tools `get_client_health`, `get_delivery_summary`, `get_report` so sales/account conversations have live delivery data (e.g. before a renewal meeting).
- **The operator's Claude** can connect to both MCP servers at once and ask cross-system questions ("which clients renew this month and how is their CPL?").

## Authentication
- Webhooks: HMAC-SHA256 signature + timestamp header, per-direction secrets stored in Vault; dedup by event id.
- REST (if needed for backfills): service credential scoped to the agency organization.
- MCP: OAuth 2.1 between the systems (service client) and per-user OAuth for humans' Claude apps. Tokens map to an organization and a principal; all permission checks apply.

## Contract to agree with Auto CRM
Event names and payload schemas (versioned), webhook endpoints and secrets, MCP tool list exposed by Auto CRM, identity fields (account id, contact ids), retry/backoff expectations, and a staging environment on both sides.
