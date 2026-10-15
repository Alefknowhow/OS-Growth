# Architecture Decision Log

## ADR-001 — Growth OS is a separate product
**Status:** Accepted

Growth OS is operationally separate from the CRM. Each system has its own application and database.

## ADR-002 — Clear source-of-truth boundaries
**Status:** Accepted

CRM owns contacts, communications, meetings, pipeline, sales and revenue. Growth OS owns paid media delivery, creatives, experiments, projects/tasks related to delivery, performance and growth learning.

## ADR-003 — Event/API integration
**Status:** Accepted

Systems exchange identifiers, events and purpose-specific data through APIs/webhooks instead of full bidirectional database synchronization.

## ADR-004 — Internal-first
**Status:** Accepted

The initial product is built for internal operation. SaaS commercialization is not an MVP requirement.

## ADR-005 — Progressive AI autonomy
**Status:** Accepted

Start with observation and recommendations. Add prepared actions with human approval before bounded automatic execution.

## ADR-006 — Modular monolith first
**Status:** Accepted

Use a modular application architecture and avoid premature microservices/Kubernetes/Kafka.

## ADR-007 — Operação de tráfego acoplada ao Althos CRM; Claude Code como motor de execução
**Status:** Accepted (supersedes ADR-001 for the operational MVP)

The traffic operation lives inside the Althos CRM traffic module (existing Supabase project). Growth OS stays as the method/architecture reference. Heavy work (strategy, copies, creative prompts, campaign assembly, weekly review) is executed by Claude Code through the CRM MCP (agent_tokens + agent_audit_log). The CRM stores data, approvals and metrics. See docs/01-product/crm-traffic-module-mapping.md.
