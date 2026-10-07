# AI Handoff

## Project
Growth OS — AI-native operating system for an automated paid traffic and acquisition agency (internal-first, multi-tenant).

## Product split
Auto CRM: agency sales and client relationship. Growth OS: delivery (media, creatives, plans, execution, performance, reports, learning). Integrated by events + MCP.

## Operating model
Agents analyze and plan → Policy Engine decides auto-execute vs approval → Execution Service performs provider writes → outcomes feed learning. Operator approves via web, mobile, voice or Claude app (MCP).

## Current phase
M0 closing. Next: M1 — Intelligence (read-only): Meta sync, signals, Performance Analyst, digest, Client Intelligence Center, Media Library v1, Growth OS MCP read tools.

## Key docs
`docs/02-architecture/first-working-loop.md`, `docs/03-ai/autonomy-policy.md`, `docs/02-architecture/operations-data-model.md`, `docs/02-architecture/performance-data-model.md`, `docs/03-ai/creative-skills.md`, `docs/04-integrations/auto-crm.md`, `.ai/DECISIONS.md`.

## Guardrails
Agents never hold provider write access. Every change is a Plan through the Policy Engine. Autonomy per client × action type, earned by track record. Consent-gated media. Never commit secrets.
