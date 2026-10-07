# Growth OS

AI-native operating system for a highly automated paid traffic and acquisition agency.

Growth OS runs delivery for every client: it ingests ad-platform data, analyzes performance, plans and executes account changes inside explicit policies, produces creatives from each client's raw media, generates reports and keeps a per-client intelligence center. The human operator stays in the **control seat**: sets policies, reviews plans, approves what needs approval (by click or by voice) and gets briefed on everything that was done.

## Operating model

```
Operator (voice / Command Center / Claude app via MCP)
        │  commands, approvals, policies
        v
Growth OS ── Data → Signals → Analysis → Plan → Policy → (Approval) → Execution → Outcome → Learning
        │                                   │
        │                                   └─ Creative: brief → skill → Higgsfield / motion graphics → QA → approval → publish
        v
Client Portal (metrics, reports, creative approvals)       Auto CRM (sales & relationship) ⇄ events + MCP
```

- **Agents analyze and plan. Policies decide what runs automatically. Humans approve the rest.** Agents never hold raw write access to ad accounts; a deterministic Execution Service performs approved actions.
- Every action is a **Plan** with evidence, expected impact, risk level and rollback, and is fully audited.
- Each client has its own **Client Intelligence Center**: campaigns, ad previews, creative and media libraries, plans, reports and profile in one place.

## Product boundary

- **Auto CRM** (sales platform): leads, prospects, conversations, meetings, proposals, contracts, billing and the commercial relationship with each client.
- **Growth OS** (delivery platform): ad accounts, media, creatives, experiments, performance, reports, operational learning.
- **Integration:** webhooks/events for state changes (e.g. deal won → client created in Growth OS) and MCP for on-demand agent access in both directions. See `docs/04-integrations/auto-crm.md`.

## Main modules

Command Center · Client Intelligence Center · Paid Media · Plans & Approvals · Creative Studio (Media Library, Creative Skills, Creative Library) · Experiments · Reports · Client Portal · Growth AI · Voice Commands. See `docs/01-product/modules.md`.

## Stack

- Next.js + TypeScript (web, API, Growth OS MCP server)
- Supabase: Postgres (RLS), Auth, Storage, Vault
- Vercel (web/API), Inngest (durable workflows), Upstash Redis (locks, rate limits)
- Render/media worker (container with ffmpeg + Chromium) for media processing and motion graphics rendering
- Claude API (agents, tool use, structured outputs); provider abstraction keeps other model vendors possible
- Speech-to-text provider behind an adapter (voice commands)
- Meta Marketing API (first), Google Ads API and GA4 Data API (later)
- MCP: Higgsfield (generative image/video), Auto CRM; Growth OS exposes its own MCP server
- Remotion (programmatic motion graphics / video composition)

## Documentation map

| Area | Start here |
|---|---|
| Vision & roadmap | `docs/00-vision/` |
| Product (modules, flows, voice, creative, portal) | `docs/01-product/` |
| Architecture & data models | `docs/02-architecture/` |
| AI (agents, tools, autonomy, skills, evals) | `docs/03-ai/` |
| Integrations (Meta, Auto CRM, Higgsfield, Remotion…) | `docs/04-integrations/` |
| Development | `docs/05-development/` |
| Decisions | `.ai/DECISIONS.md` |

## Status

**M0 — Foundation** (architecture complete, implementation next). Next: **M1 — Intelligence (read-only)**. See `docs/00-vision/roadmap.md` and `docs/02-architecture/first-working-loop.md`.
