# Voice Commands

The operator can run the agency by voice: ask, command, request creatives and approve plans.

## Channels (in rollout order)
1. **Claude app voice + Growth OS MCP server (M1):** the operator talks to Claude (desktop/mobile) connected to the Growth OS MCP server. Zero custom voice infrastructure; read-only in M1, plan proposal/approval tools in M2.
2. **Command Center push-to-talk (M2):** native mic in web/PWA, client-scoped or global.
3. **Later:** messaging voice notes (e.g. WhatsApp) through a verified operator channel.

## Pipeline (native channel)
```
audio → speech-to-text → Command Interpreter (structured output) → resolver → router
                                                        │
          query ─────────────── answer (text + optional spoken summary)
          plan_request ──────── agent drafts Plan → Policy Engine → (auto | approval inbox)
          approval ──────────── approve / reject plan (with read-back)
          creative_request ──── Creative workflow (brief → skill → job)
          report_request ────── Reporting workflow
          task ──────────────── create task
```

## Command structure
The Command Interpreter returns: `intent`, `client` (resolved id + confidence), `entities` (campaign, ad set, ad, creative, period), `parameters` (amounts, percentages, formats), `confidence`, `clarification_needed`.
The resolver fuzzy-matches names against the client's entities. Ambiguity → ask a short clarifying question instead of guessing.

## Examples (pt-BR)
- "Como está a Clínica Sorriso essa semana?" → query.
- "Pausa os anúncios com CPL acima de 40 reais na campanha de implante." → plan_request.
- "Sobe 20% o orçamento do conjunto de remarketing da Dra. Ana." → plan_request.
- "Faz três criativos em vídeo pra oferta de clareamento, formato reels, usando os vídeos que ela gravou semana passada." → creative_request.
- "Aprova o plano dois." / "Rejeita, o cliente pediu pra segurar verba." → approval with reason.
- "Gera o relatório mensal da Clínica Sorriso e publica no portal." → report_request.

## Safety rules
- Voice never executes provider calls directly. Every change becomes a Plan and passes the Policy Engine — same rules as any other origin.
- Voice approval: the system reads back the plan summary (client, action, amounts) and requires an explicit confirmation phrase.
- `high`/`critical` risk plans require on-screen confirmation or step-up auth (passkey/biometric) even when requested by voice.
- Commands are attributed to the authenticated user and stored with transcript (audio retention configurable) for audit.
- Content heard or read from external sources is never treated as a command.
