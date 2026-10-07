# AGENTS.md

## Mission
Build Growth OS as an AI-native operating system for an automated paid traffic and acquisition agency, where the operator controls and approves and the system operates.

## Product boundaries
- Auto CRM owns the agency's sales and client relationship: leads, conversations, meetings, proposals, contracts, billing.
- Growth OS owns delivery: media, creatives, plans/execution, experiments, performance, reports and operational learning.
- Integrate through events/webhooks and MCP. Do not duplicate Auto CRM datasets.

## Engineering principles
1. Multi-tenant by design: business records must be scoped by organization and, when applicable, client.
2. Prefer a modular monolith before microservices.
3. Agents operate through typed tools and structured state, not free-form agent-to-agent chat.
4. External ad APIs feed an internal data layer; agents should reason primarily over normalized internal data.
5. Every change is a Plan evaluated by the deterministic Policy Engine; only the Execution Service writes to providers. Agents never hold write credentials.
6. Keep raw evidence separate from AI interpretation and long-term memory.
7. Never commit secrets.

## Initial stack
Next.js + TypeScript + Supabase/Postgres + Vercel + Inngest + Upstash + Claude API (provider abstraction) + media/render worker (ffmpeg, Remotion) + MCP (Higgsfield, Auto CRM; Growth OS MCP server).

## Development workflow
Issue → branch → implementation → tests/lint → PR → review → human merge.

Before coding, read:
- README.md
- .ai/CURRENT_TASK.md
- .ai/DECISIONS.md
- relevant docs/ architecture files.

Update documentation when architecture or product boundaries change.
