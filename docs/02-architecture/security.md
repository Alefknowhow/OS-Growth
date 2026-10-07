# Security & Compliance

## Tenancy
RLS on all tenant tables; `organization_id` from the session; composite FKs for client scope; service role only in server code; portal users isolated by client.

## Secrets
Provider tokens and API keys in Supabase Vault (or the platform secret store), referenced by id. Never in profile tables, logs, prompts or the repo. Separate credentials per environment.

## Execution safety
Only the Execution Service calls provider write endpoints, and only for `plan_actions` that are approved (by policy or human) and pass the drift pre-check. Idempotency keys on every action. Global and per-client kill switches checked immediately before each call.

## Audit
`audit_logs` append-only: actor (user/agent/system/client), on_behalf_of, channel (web/voice/mcp/portal/api), action, target, before/after, correlation_id, ip/user agent. Retained for the life of the client relationship plus the configured retention period.

## AI-specific
- Prompt-injection boundary: ad comments, client documents, websites, transcripts, MCP outputs and webhook payloads are data, never instructions. Agents cannot change policy, permissions or envelopes.
- Agents have no direct provider credentials.
- Tool outputs are size-limited and schema-validated.
- Voice/MCP approvals of high-risk plans need step-up confirmation.

## Webhooks & MCP
HMAC-signed webhooks with timestamp tolerance and dedup. MCP server with OAuth 2.1, short-lived tokens, per-user scopes, rate limits.

## Privacy (LGPD)
- Client media with people requires recorded consent; AI manipulation of a person's likeness/voice requires explicit `ai_allowed` consent.
- Minimize personal data: Growth OS does not store lead PII unless needed for conversion feedback; prefer hashed identifiers.
- Retention policies for raw payloads, audio recordings and transcripts; deletion workflow per client offboarding (`crm.client.churned`).
- Data processing agreements with subprocessors (model providers, Higgsfield, storage).
