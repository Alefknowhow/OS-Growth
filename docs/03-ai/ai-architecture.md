# AI Architecture

## Operating model
- Deterministic code detects (signals), governs (policy), executes (Execution Service) and orchestrates (Inngest workflows).
- LLM agents interpret, plan, write and create. They consume **context packs**, call **typed tools**, and return **structured outputs** (insights, plans, briefs, copy, reports) validated against schemas before persistence.
- Agents never call provider write APIs and never hold provider credentials.

## Closed loop
Data → Signals → Insight → Plan → Policy → (Approval) → Execution → Verification → Outcome → Learning → (better context next time).

## Runtime
| Workload | Runtime | Why |
|---|---|---|
| Analyst, Media Buyer, Strategist, Content, Reporting, Command Interpreter, Onboarding | Claude API with tool use (SDK Tool Runner or manual loop) inside Inngest steps | Growth OS hosts every tool, so each call is permission/policy/audit checked; durable retries |
| Creative Producer | Claude API tool loop in the media worker, with Growth OS tools wrapping Higgsfield (MCP client), Remotion and ffmpeg | needs binaries and long renders; cost tracking per step |
| Exploratory creative R&D (optional) | Claude Managed Agents session (sandbox + Skills + MCP) | evaluate in an M3 spike for ad-hoc motion graphics authoring; production paths stay on versioned skills |

## Models
- Default for all agent roles: `claude-opus-5-5` with adaptive thinking; effort tuned per route (higher for planning/creative strategy, lower for parsing/tagging) after evals.
- Per-role model changes (e.g. a smaller model for asset tagging or command parsing) only when evals show quality holds.
- A provider abstraction (`ai/providers`) keeps model choice configurable per agent and allows other vendors (e.g. for speech-to-text).
- Handle `refusal` stop reasons and configure fallbacks per the current Claude API guidance.

## Structured outputs
Every agent output that becomes a record uses a JSON schema (structured outputs / strict tools). Invalid output → retry with the validation error → fail the run visibly (never persist partial artifacts).

## Context packs
Deterministic builders (`performance_analysis`, `media_buying`, `creative_strategy`, `content_generation`, `creative_production`, `reporting`, `command_resolution`, `experiment_review`) define required sections, data limits and freshness. The exact pack is stored in `context_snapshots` for every run. Stable parts (system prompt, tool definitions, client profile summary) are ordered first to benefit from prompt caching.

## Versioning & observability
Prompts, skills and context-pack builders are versioned in the repo. Each `ai_run` records model, prompt version, skill version, context snapshot, tool calls, tokens, cost, latency and outcome. Changes to prompts/skills must pass evals (see `evals-and-feedback.md`).

Related: `agents.md`, `orchestrator.md`, `tools.md`, `autonomy-policy.md`, `memory.md`, `signal-detection.md`, `creative-skills.md`.
