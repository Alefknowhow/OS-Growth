# Evals & Feedback

Autonomy can only widen if quality is measured.

## Online signals (production)
| Metric | Source |
|---|---|
| Insight acceptance rate, dismissal reasons | recommendation_feedback |
| Plan approval rate, edit rate, rejection reasons | approvals, recommendation_feedback |
| Action outcome verdicts (improved/neutral/worse) | action_outcomes |
| Rollback rate | action_executions |
| Signal precision (signals leading to accepted insights) | signals ↔ insights |
| Creative first-pass approval rate, revision count | creative_reviews |
| Creative performance vs account baseline | v_creative_performance |
| Report edits before publish | reports |
| Command resolution accuracy (clarifications, corrections) | commands |
| Operator minutes per client per week, approval latency | UI telemetry |
| Cost per run / per job / per client | ai_runs, creative_jobs |

## Offline evals
- Per agent, an eval set built from real cases (anonymized where needed): input context snapshot + expected properties (not exact text).
- Graders: deterministic checks (schema, policy compliance, numbers match data) + model-graded rubrics for quality, with periodic human calibration.
- Required on any change to prompts, skills, context-pack builders, detectors or model/effort settings; results stored in `eval_runs` and linked to the PR.

## Feedback capture
Every rejection/edit asks for a quick reason tag (wrong diagnosis, too aggressive, client constraint, bad timing, off-brand, factual error) plus optional note. Reasons are reviewed weekly to update prompts, policies or detectors.
