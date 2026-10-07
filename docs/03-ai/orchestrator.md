# Growth Orchestrator

The orchestrator coordinates work; it is not a universal expert. It is implemented as **deterministic Inngest workflows** (ADR-008).

## Workflows
| Workflow | Trigger | Steps |
|---|---|---|
| `daily_client_review` | `meta.sync.completed` (daily) | detect signals → Analyst → Media Buyer → policy → auto-execute / request approval |
| `intraday_guard` | intraday sync | pacing/zero-delivery/overspend detectors → guardrail plans (e.g. pause on runaway spend) |
| `command_handling` | `command.received` | interpret → resolve → route (query / plan / creative / report / approval) |
| `plan_execution` | `plan.approved` | per action: kill switch → drift check → execute → verify → audit → schedule outcome eval |
| `outcome_evaluation` | scheduled after action window | compute verdict → update track record |
| `creative_production` | `creative.requested` | brief → skill → producer → QA → review request |
| `creative_publish` | `creative.approved` | Media Buyer drafts publish plan → policy |
| `report_generation` | schedule / command | snapshot → Reporting agent → review/publish |
| `client_onboarding` | `crm.deal.won` / manual | create client → facts from Auto CRM → checklist |
| `daily_digest` | schedule per operator | collect → summarize → deliver |

## Responsibilities
- Assemble the context pack for each agent.
- Enforce permissions and policy; never let an agent bypass the Policy Engine.
- Concurrency control: one planning run per client at a time; one execution per ad account at a time.
- Deduplicate (signal dedup keys; no new plan if an equivalent one is pending).
- Expire stale plans (data older than the plan's `data_as_of` tolerance).
- Record provenance (correlation ids across the whole loop).
