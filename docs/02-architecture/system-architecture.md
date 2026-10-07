# System Architecture

## High-level
```
                 Operator: Web/PWA · Voice · Claude app (MCP)          Client Portal
                                   │                                         │
                                   v                                         v
┌──────────────────────────── Growth OS (modular monolith, Next.js) ─────────────────────────────┐
│  UI / API  ·  Growth OS MCP server  ·  Command Interpreter                                       │
│                                                                                                  │
│  Domain modules: clients/profile · paid-media · plans · policy · execution · creative ·         │
│                  media-library · experiments · reports · tasks · audit                          │
│                                                                                                  │
│  AI layer: agents (Claude API + typed tools) · context packs · skills · runs/evals              │
│                                                                                                  │
│  Workflows (Inngest): sync · detect · analyze · plan · execute · creative jobs · reports        │
└───────┬───────────────────────┬─────────────────────────┬──────────────────────┬──────────────┘
        │                       │                         │                      │
        v                       v                         v                      v
 Provider adapters       Supabase (Postgres+RLS,    Media/Render worker     Outbox → Events/Webhooks
 Meta · Google · GA4     Auth, Storage, Vault)      ffmpeg · Remotion ·          │
 Higgsfield (MCP)        Upstash Redis (locks,      transcripts                  v
 Auto CRM (MCP/API)      rate limits)                                       Auto CRM
```

## Deployables
| Unit | Runs | Why separate |
|---|---|---|
| Web/API (Vercel) | Next.js UI, API routes, Growth OS MCP server, webhook receivers | standard request/response |
| Workflows (Inngest functions on Vercel) | sync, detection, agent runs, execution, reports | durable, retryable steps |
| Media/Render worker (container) | media processing, transcripts, Remotion renders, ffmpeg | long-running, CPU/GPU, binaries not suited to serverless |

Everything else is one codebase with module boundaries.

## Module boundaries
- `integrations/*` — provider adapters only; translate between provider APIs and internal types.
- `domain/*` — business rules, persistence, invariants (no LLM calls).
- `policy` — pure, deterministic evaluation of proposed actions.
- `execution` — the only module allowed to call provider write endpoints.
- `ai/*` — agents, tools, context packs, skills; tools call domain services, never providers directly.
- `workflows/*` — Inngest orchestration composing the above.
- `ui/*`, `mcp/*` — presentation and external agent interface; both go through the same domain services and permission checks.

## Key flows
- **Read path:** adapters → raw ingestion → normalized tables → signals → agents.
- **Write path:** agent/command → Plan → Policy Engine → (approval) → Execution Service → provider → verification → audit → outcome.
- **Creative path:** brief → skill → worker/Higgsfield/Remotion → storage → QA → review → publish plan.
- **Integration path:** domain change → outbox → Inngest → outbound webhook/MCP; inbound webhook → verify → dedupe → event → handler.

Related: `database.md`, `performance-data-model.md`, `operations-data-model.md`, `creative-data-model.md`, `event-architecture.md`, `mcp-architecture.md`, `security.md`.
