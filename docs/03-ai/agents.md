# Agents

Each agent has a narrow job, a context pack, an allowed tool set and a structured output. Agents don't chat with each other; workflows pass typed artifacts between them.

| Agent | Input | Output | Tools (classes) | Phase |
|---|---|---|---|---|
| Command Interpreter | transcript/text + user scope | `ParsedCommand` | client/entity resolution (read) | M1 (MCP) / M2 (native) |
| Performance Analyst | signals + metrics + profile | `insights[]` | read metrics, profile, learnings | M1 |
| Media Buyer | insights, goals, budget, envelope, history | `plans[]` with typed actions | read + `propose_plan` | M2 |
| Creative Strategist | profile, creative performance, learnings, request | `creative_brief` / hypotheses | read + `create_brief` | M3 |
| Content Agent | brief, persona, tone, claims | copy, scripts, captions | read profile/claims | M1–M3 |
| Creative Producer | brief + skill | creatives (assets + copy) | media search, Higgsfield, Remotion render, ffmpeg (all wrapped) | M3 |
| Creative QA | creative + brand kit + claims + specs | `qa_report` pass/fail | read assets, frame sampling, OCR/transcript | M3 |
| Reporting Agent | period data snapshot, goals, actions, learnings | report narrative + sections | read | M2 |
| Onboarding/Context Agent | documents, website, Auto CRM summaries | proposed facts | `propose_client_fact`, knowledge search | M1 |
| Experiment Analyst | experiment data | results, conclusion, proposed learning | read + `propose_learning` | M5 |

## Growth Orchestrator
Not a free-form agent. It is the set of Inngest workflows that trigger agents, assemble context packs, route outputs, enforce policy and request approvals. An LLM is used inside it only to route ambiguous commands. See `orchestrator.md`.

## Execution Service (not an agent)
Deterministic code that executes approved plan actions. See `autonomy-policy.md`.

## Agent contract (each agent spec defines)
Purpose · trigger · context pack · allowed tools · output schema · quality bar / eval set · failure behavior · cost budget per run.
