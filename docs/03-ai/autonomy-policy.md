# Autonomy & Policy

Goal: the system operates accounts on its own **inside explicit limits**, and the operator only approves what falls outside them. Autonomy is configured per client and per action type, and widens with measured track record.

## Levels (per client × action type)
0. Observe — read only.
1. Recommend — insights only.
2. Prepare — plans require human approval.
3. Bounded auto — plans inside the autopilot envelope execute automatically and appear in the executed feed with undo.
4. Extended auto — wider envelope granted after track record (still bounded; never unlimited).

Defaults for a new client: guardrail actions (e.g. pause on runaway spend) at level 3; everything else at level 2.

## Policy Engine
Pure, deterministic function: `(plan_action, client state, envelope, policy_rules, actor) → decision + reasons + risk_level`. No LLM involvement. Evaluated when the plan is proposed **and again** right before execution.

Inputs considered:
- Action type and its level for this client.
- Magnitude: absolute and relative change (e.g. budget +15%, +R$ 40/day).
- Cumulative change in the period (daily/weekly caps across all actions).
- Money at stake (projected spend impact).
- Entity state (learning phase, recent changes / cooldown, spend level).
- Data quality (open `data_quality` or `tracking_break` signal blocks automatic execution).
- Time rules (quiet hours, end-of-month, client blackout dates).
- Actor permissions (on-behalf-of user ceiling).

## Autopilot envelope (per client, versioned)
Example defaults (all configurable):
| Rule | Default |
|---|---|
| Max budget increase per action | +20% |
| Max budget decrease per action | −30% |
| Max cumulative budget change per entity per 7 days | ±40% |
| Max daily spend increase per client per day | R$ value set at onboarding |
| Never exceed monthly client budget | hard |
| Cooldown between budget changes on same entity | 72h |
| Pause ads | allowed if rule-based (e.g. spend ≥ 2× target CPA with 0 conversions) |
| Activate paused entities | approval |
| Duplicate ad sets / create ads | approval (until track record) |
| Targeting / structure changes | approval |
| Max automatic actions per client per day | 10 |

Risk levels derive from rules: `low` (inside envelope), `medium` (≤2× envelope limits), `high` (above or structural), `critical` (large money at stake or affects whole account).

## Graduation (earning autonomy)
Track record per client × action type: number of executed actions, share with outcome `worse`, rollbacks, operator rejections of similar plans. The system **suggests** widening (or narrowing) the envelope; only owner/admin can change it. Any rollback due to error or a `worse` streak automatically narrows to level 2 for that action type.

## Execution Service guarantees
1. Kill switch check (global, client).
2. Policy re-evaluation.
3. Drift check: live entity state must match `before_snapshot` on relevant fields; otherwise skip and replan.
4. Idempotent provider call with idempotency key.
5. Read-back verification; mismatch → alert.
6. Append-only execution record + audit log + `entity_changes` entry.
7. Schedule outcome evaluation.
8. Rollback action available in the executed feed.

## Approvals
Role ceilings per risk level (see `../01-product/permissions.md`); optional two-person rule for critical; plans expire; voice/MCP approvals need read-back and step-up for high/critical.

## Hard limits (never automatic, never by agents)
Deleting entities, billing/payment methods, account-level settings, pixel/dataset/conversion configuration, user access, special ad category changes, anything without a rollback path at level 3+.
