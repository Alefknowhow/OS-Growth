# System Architecture

## High-level
```
Meta Ads ─┐
Google Ads ├─> Integration Workers ─> Growth Data Layer ─> AI/Decision Layer
GA4 ──────┘                                      │               │
                                                 │               v
CRM <──────────── API / Webhooks / Events ───────┘        Tasks/Experiments
                                                                  │
                                                                  v
                                                         Approved Execution
```

## Application
Start as a modular monolith:
- Web/UI: Next.js
- API/domain services: Next.js server layer
- Database/Auth/Storage: Supabase
- Durable workflows: Inngest
- Cache/locks/rate coordination: Upstash Redis
- Deployment: Vercel

## Key boundaries
Integrations, domain logic, AI tools, agent orchestration and UI must remain separable modules even when deployed together.
