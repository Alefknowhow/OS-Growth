# Event Architecture

Inngest runs durable workflows. Domain changes are published through a transactional outbox so events are never lost or emitted for rolled-back transactions.

## Envelope (all events)
```json
{
  "id": "evt_…",                 // unique, used for dedup
  "name": "plan.approved",
  "version": 1,
  "occurred_at": "2026-10-07T12:00:00Z",
  "organization_id": "…",
  "client_id": "…",              // when applicable
  "actor": { "type": "user|agent|system|client|external", "id": "…" },
  "correlation_id": "…",         // ties a whole loop together
  "causation_id": "…",           // event/command that caused this one
  "idempotency_key": "…",
  "data": { }
}
```
Payloads carry ids and minimal data; consumers read details through domain services.

## Naming
`<domain>.<entity>.<past_tense_verb>`, versioned by `version`. Breaking payload changes bump the version; consumers handle both during migration.

## Catalog (initial)
| Domain | Events |
|---|---|
| Client lifecycle | `client.created`, `client.onboarding.completed`, `client.readiness.changed`, `client.autopilot.changed`, `client.kill_switch.toggled` |
| Integrations | `connection.created`, `connection.health.changed`, `meta.sync.completed`, `meta.sync.failed`, `meta.external_change.detected` |
| Performance | `signal.detected`, `signal.resolved` |
| AI loop | `insight.created`, `plan.proposed`, `plan.approval.requested`, `plan.approved`, `plan.rejected`, `plan.expired`, `action.executed`, `action.failed`, `action.rolled_back`, `action.outcome.evaluated` |
| Commands | `command.received`, `command.resolved` |
| Media & creative | `asset.uploaded`, `asset.processed`, `creative.requested`, `creative_job.completed`, `creative_job.failed`, `creative.review.requested`, `creative.approved`, `creative.rejected`, `creative.published` |
| Experiments | `experiment.launched`, `experiment.completed`, `learning.proposed`, `learning.validated` |
| Reporting | `report.generated`, `report.published` |
| Inbound from Auto CRM | `crm.deal.won`, `crm.contract.updated`, `crm.client.churned`, `crm.meeting.processed`, `crm.contact.updated` |
| Outbound to Auto CRM | `growth.client.onboarded`, `growth.report.published`, `growth.client.risk_detected`, `growth.approval.needed_from_client`, `growth.client.health.changed` |

## Inbound webhooks
`inbound_webhooks` table stores every request (headers, body, signature result, received_at). Signature (HMAC) and timestamp are verified; duplicates are ignored by provider event id; valid requests become internal events.

## Outbound webhooks
Dispatched by Inngest from the outbox with HMAC signatures, retries with backoff and a dead-letter state visible in the UI.

## Rules
- Tenant-scoped, idempotent consumers, observable (every event links to its workflow runs).
- Inngest concurrency keys per ad account for sync/execution and per client for planning, preventing conflicting concurrent plans.
