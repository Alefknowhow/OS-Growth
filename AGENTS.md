# AGENTS.md

## Mission
Build Growth OS as an AI-native operating system for paid media and growth operations.

## Product boundaries
- CRM owns relationship, conversations, meetings, opportunities, sales and revenue.
- Growth OS owns delivery, media, creatives, experiments, performance and operational learning.
- Integrate through APIs/events. Do not duplicate entire CRM datasets.

## Engineering principles
1. Multi-tenant by design: business records must be scoped by organization and, when applicable, client.
2. Prefer a modular monolith before microservices.
3. Agents operate through typed tools and structured state, not free-form agent-to-agent chat.
4. External ad APIs feed an internal data layer; agents should reason primarily over normalized internal data.
5. Read/recommend before write/execute. All consequential actions need policies, permissions and auditability.
6. Keep raw evidence separate from AI interpretation and long-term memory.
7. Never commit secrets.

## Initial stack
Next.js + TypeScript + Supabase/Postgres + Vercel + Inngest + Upstash + OpenAI/Anthropic.

## Development workflow
Issue → branch → implementation → tests/lint → PR → review → human merge.

Before coding, read:
- README.md
- .ai/CURRENT_TASK.md
- .ai/DECISIONS.md
- relevant docs/ architecture files.

Update documentation when architecture or product boundaries change.
