# Architecture Decision Log

## ADR-001 — Growth OS is a separate product
**Status:** Accepted

Growth OS is operationally separate from the CRM. Each system has its own application and database.

## ADR-002 — Clear source-of-truth boundaries
**Status:** Accepted (CRM = Auto CRM, see ADR-012)

CRM owns contacts, communications, meetings, pipeline, sales and revenue. Growth OS owns paid media delivery, creatives, experiments, projects/tasks related to delivery, performance and growth learning.

## ADR-003 — Event/API integration
**Status:** Accepted

Systems exchange identifiers, events and purpose-specific data through APIs/webhooks instead of full bidirectional database synchronization.

## ADR-004 — Internal-first
**Status:** Accepted

The initial product is built for internal operation. SaaS commercialization is not an MVP requirement.

## ADR-005 — Progressive AI autonomy
**Status:** Superseded by ADR-007

Start with observation and recommendations. Add prepared actions with human approval before bounded automatic execution.

## ADR-006 — Modular monolith first
**Status:** Accepted

Use a modular application architecture and avoid premature microservices/Kubernetes/Kafka.

## ADR-007 — Plan-based, policy-governed autonomy
**Status:** Accepted

Every change is a Plan of typed actions. A deterministic Policy Engine classifies each action as auto-execute (inside the client's autopilot envelope), approval-required or forbidden. Autonomy is set per client × action type and widens with measured track record. Goal: the operator controls and approves; the system operates. See `docs/03-ai/autonomy-policy.md`.

## ADR-008 — Deterministic orchestration
**Status:** Accepted

The Growth Orchestrator is a set of Inngest workflows, not a free-form LLM agent. LLMs are used for interpretation, planning and creation inside steps; routing uses an LLM only for ambiguous commands.

## ADR-009 — Rule-based signal detection before LLM analysis
**Status:** Accepted

Deterministic detectors produce signals with evidence; agents interpret signals. Cheaper, reproducible and testable.

## ADR-010 — Agents never hold write access
**Status:** Accepted

Only the Execution Service calls provider write APIs, after policy/approval, drift check and kill-switch check, with idempotency, read-back verification, audit and rollback.

## ADR-011 — Facts are the review layer; profile tables are the confirmed state
**Status:** Accepted

AI and integrations write `client_facts`; confirmation applies values to profile tables via a field map. Historical reproduction uses `context_snapshots` per AI run instead of a temporal profile.

## ADR-012 — Auto CRM integration via events + MCP
**Status:** Accepted

Auto CRM is the agency's sales/relationship platform. State changes flow through signed webhooks/events (e.g. `crm.deal.won` creates the client); agents use MCP in both directions for on-demand context. No dataset duplication. See `docs/04-integrations/auto-crm.md`.

## ADR-013 — Growth OS exposes an MCP server
**Status:** Accepted

Read tools in M1, propose/approve tools in M2, creative tools in M3. OAuth per user; same permissions and policy as the UI. Gives voice operation through the Claude app before native voice exists.

## ADR-014 — MCP tools are wrapped as Growth OS tools
**Status:** Accepted

Remote MCP tools (Higgsfield, Auto CRM) are called through Growth OS's own MCP client and exposed to agents as typed tools, so every call is permission-checked, cost-tracked and audited. Direct API-side MCP connectors only for low-risk read-only prototyping.

## ADR-015 — Creative production through versioned skills
**Status:** Accepted

Each creative format is a versioned skill (SKILL.md + spec + QA + Remotion compositions) executed by the Creative Producer with Higgsfield, Remotion and ffmpeg tools. New formats are added as reviewed skills, not ad-hoc prompts.

## ADR-016 — Separate media/render worker
**Status:** Accepted

Media processing and video rendering run in a container worker (ffmpeg, Chromium/Remotion), not in serverless functions. Exception to "single deployable" justified by binaries and long runtimes.

## ADR-017 — Voice is a channel, not a bypass
**Status:** Accepted

Voice commands produce the same Commands/Plans as any channel and pass the same policy. Voice approvals require read-back; high/critical risk requires step-up confirmation.

## ADR-018 — Consent-gated media usage
**Status:** Accepted

Assets with people require recorded usage and likeness consent; AI manipulation of a person requires explicit `ai_allowed` consent. Skills refuse non-compliant assets.
