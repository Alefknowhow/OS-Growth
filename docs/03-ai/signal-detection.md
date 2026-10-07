# Signal Detection

Deterministic detectors run after each sync and write `signals`. LLM agents interpret signals; they don't scan raw metrics to find problems. This keeps detection cheap, reproducible and testable.

## Detectors (initial)
| Detector | Logic (defaults configurable per client) | Min sample |
|---|---|---|
| `pacing` | MTD spend vs linear (or seasonal) budget plan deviates > ±15% | — |
| `budget_exhausted` | campaign/ad set hit budget early in the day repeatedly | 3 days |
| `cpa_spike` | CPA (primary conversion) 3-day vs 14-day baseline > +35% | ≥ 10 conversions baseline |
| `spend_no_conversions` | spend ≥ 2× target CPA with 0 conversions | — |
| `ctr_drop` | link CTR 3-day vs 14-day baseline < −30% | ≥ 5k impressions |
| `creative_fatigue` | frequency ↑ and CTR ↓ over 7 days on an ad with meaningful spend | ≥ 7 days running |
| `zero_delivery` | active entity with 0 impressions for N hours | — |
| `learning_limited` | ad set flagged learning limited | — |
| `tracking_break` | conversions drop to ~0 while clicks/LPV stay normal | — |
| `goal_off_track` | projected period KPI misses target by > X% | — |
| `opportunity_scale` | CPA well below target with stable volume and headroom | ≥ 20 conversions |
| `data_quality` | reconciliation failures, missing days | — |

## Rules
- Each signal stores baseline, observed value, window, sample size and the evidence query.
- `dedup_key` (detector + entity + window) prevents repeats; signals auto-resolve when the condition clears.
- Severity from magnitude × money at stake.
- Detector versions are recorded; changes are tested against historical data before rollout.
