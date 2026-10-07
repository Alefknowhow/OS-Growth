# First Working Loops (M1 and M2)

Concrete end-to-end slices that prove the architecture.

## M1 — "Know every account every morning" (read-only)
1. Operator creates a client (or `crm.deal.won` stub creates it) and fills the minimum profile.
2. Meta ad account (shared to the agency Business Manager) is linked; backfill runs.
3. Nightly + hourly sync → `metric_daily`, `metric_intraday`, `entity_changes`.
4. `meta.sync.completed` → signal detectors → `signals`.
5. Performance Analyst runs per client with the `performance_analysis` context pack → `insights` (structured, evidence-linked).
6. Daily digest assembled at 07:00 local time (signals + insights per client).
7. Client Intelligence Center shows overview, campaigns, campaign detail with ad previews, insights.
8. Growth OS MCP server (read tools) → operator asks by voice in the Claude app: "Como estão meus clientes hoje?"

**Done when:** for every connected client, the operator gets a correct morning digest without opening Ads Manager, and metrics reconcile with Ads Manager within tolerance.

## M2 — "Approve, don't operate"
1. Media Buyer agent converts insights into `plans` with typed `plan_actions` (pause ad, adjust ad set budget, reallocate budget, duplicate ad set, create ad from approved creative).
2. Policy Engine classifies each action against policy rules and the client's autopilot envelope.
3. Inside envelope → executes; otherwise → Approval Inbox (web, mobile, voice with read-back).
4. Execution Service: kill-switch check → drift pre-check → provider call → read-back verification → `action_executions` → audit.
5. Outcome evaluation after window → `action_outcomes` → track record.
6. Digest lists executed, pending and rolled back actions.

**Done when:** a week of operation on real clients where every account change was made by the Execution Service, with zero unauthorized or unverifiable changes, and the operator's time is spent approving rather than executing.

## M3 preview — "Ask for creatives, get creatives"
"Faz 3 reels pra oferta X com os vídeos dela" → brief → `ugc-cut-reels` skill → assets → copy → Remotion render (+ Higgsfield b-roll) → QA → internal review → publish plan.
