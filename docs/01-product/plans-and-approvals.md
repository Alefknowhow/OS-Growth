# Plans & Approvals

Everything the system wants to change becomes a **Plan**. This is how the operator stays in control without doing the work.

## Plan
- Title and summary in plain language ("Shift R$ 50/day from Ad Set A to Ad Set B").
- Origin: signal, scheduled review, operator command (voice/text), agent workflow, client request.
- Rationale and evidence (metrics, signals, insights, learnings referenced).
- Expected impact and how it will be measured (KPI, window).
- Risk level: `low | medium | high | critical` (computed by the Policy Engine, not by the LLM).
- Proposed actions (one or more typed actions, each with before-state snapshot and rollback).
- Policy decision per action: `auto_execute | approval_required | forbidden` with reasons.
- Expiry (plans based on stale data expire).

## Lifecycle
`draft → pending_approval → approved → executing → executed | partially_executed | failed → (rolled_back)`; also `rejected`, `expired`, `superseded`.
Plans fully inside the autopilot envelope go `draft → approved (by policy) → executing`.

## Approval Inbox
- Sorted by risk, expiry and money at stake.
- One-tap approve / reject / edit parameters / ask agent "why?".
- Batch approval for low-risk homogeneous plans.
- Voice approval with read-back (see `voice-commands.md`).
- Mobile-friendly (PWA) and push notifications for high-risk or expiring plans.

## After execution
- Executed feed with undo (rollback action) when available.
- Outcome evaluation after the measurement window: improved / neutral / worse / inconclusive.
- Outcomes feed the track record that can widen or narrow the autopilot envelope.

## Daily digest
Morning summary per operator: what was executed automatically, what awaits approval, top risks, creative pipeline, reports due. Available as text and as a short audio summary.

## Feedback
Rejections and edits capture a reason (quick tags + optional note). This is the main training signal for agents and policies (see `docs/03-ai/evals-and-feedback.md`).
