# Operations Data Model (the AI loop)

Typed artifacts that connect detection → analysis → planning → execution → outcome → learning. Every row carries `organization_id` and `client_id` (when applicable).

## Detection & analysis
### signals (deterministic)
id, type (`pacing`, `cpa_spike`, `ctr_drop`, `creative_fatigue`, `zero_delivery`, `learning_limited`, `spend_no_conversions`, `tracking_break`, `budget_exhausted`, `goal_off_track`, `data_quality`, `opportunity_scale`), entity_type, entity_id, metric, window, baseline, observed, delta, sample_size, severity, evidence (jsonb: query + values), detector_version, dedup_key, status (`open|acknowledged|resolved|suppressed`), detected_at.

### insights (AI interpretation)
id, ai_run_id, kind (`problem|opportunity|risk|observation`), title, diagnosis, signal_ids[], evidence_refs, confidence, status (`open|accepted|dismissed|resolved`), expires_at.

### hypotheses
id, insight_id nullable, statement, rationale, expected_effect, kpi, priority, status, experiment_id nullable.

## Planning & execution
### plans
id, origin (`signal|command|schedule|agent|client_request|experiment`), origin_ref, title, summary, rationale, evidence_refs, expected_impact (jsonb: kpi, direction, magnitude, window), risk_level, status, created_by_type (`agent|user`), created_by_id, ai_run_id, data_as_of, expires_at, superseded_by.

### plan_actions
id, plan_id, sequence, action_type (from the action catalog, e.g. `meta.ad.set_status`, `meta.adset.update_budget`), target (provider + entity ids), params (jsonb, schema per action type), before_snapshot, policy_decision (`auto_execute|approval_required|forbidden`), policy_reasons, risk_level, idempotency_key, rollback (jsonb action spec or null), status.

### approvals
id, plan_id, plan_action_id nullable, required_role/risk ceiling, decided_by, decision (`approved|rejected|changes_requested`), channel (`web|mobile|voice|mcp|portal`), command_id nullable, reason_tags, comment, decided_at.

### action_executions (append-only)
id, plan_action_id, attempt, started_at, finished_at, pre_check (drift result), request_ref, response_ref, result (`succeeded|failed|skipped_drift|skipped_policy`), error_class, verification (read-back result), correlation_id.

### action_outcomes
id, plan_action_id, kpi, window, before_value, after_value, control_ref nullable, verdict (`improved|neutral|worse|inconclusive`), method, evaluated_at.

### autopilot_envelopes / policy_rules
See `../03-ai/autonomy-policy.md`. Envelopes are versioned; every policy decision records the envelope version used.

### commands
id, user_id, channel, transcript, audio_ref nullable, parsed (jsonb), client_id resolved, status (`parsed|clarifying|routed|done|failed`), result_ref (answer, plan, job, report).

### recommendation_feedback
id, target_type (`insight|plan|creative|report`), target_id, decision, edits (diff), reason_tags, comment, user_id.

## Learning
### experiments / experiment_variants / experiment_results
hypothesis_id, design (variable, variants, KPI, budget, duration, success criteria), plan_id for launch, status, results with statistical summary, conclusion.

### learnings
id, scope (`client|vertical|organization`), category (`creative_angle|hook|audience|offer|bidding|budget|landing|seasonality`), statement, evidence_refs (experiments, outcomes, metrics), confidence, status (`proposed|validated|deprecated`), valid_from, valid_until, reviewed_by.

## Work & reporting
projects, tasks (assignee user or agent, source ref, due date, status), reports (period, type, data_snapshot_ref, narrative, status `draft|in_review|published`, published_to_portal_at), report_schedules.

## AI observability
- **ai_runs**: agent, workflow_run_id, trigger ref, model, prompt_version, skill_version, context_pack, context_snapshot_id, input_refs, output_ref, structured_output, tokens_in/out, cost, latency, status, correlation_id.
- **context_snapshots**: exact context pack content (or storage ref) + hash — replaces the need for an `as_of` temporal profile for reproducing past runs.
- **ai_tool_calls**: ai_run_id, tool, input, output_ref, classification (`read|propose|execute|external_generate`), authorized, duration, error.
- **prompt_versions**, **eval_runs**: see `../03-ai/evals-and-feedback.md`.
