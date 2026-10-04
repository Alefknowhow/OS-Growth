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
