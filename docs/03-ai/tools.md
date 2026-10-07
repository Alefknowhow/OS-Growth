# AI Tool Layer

Canonical tool catalog. Tools are typed (input/output schemas), tenant-scoped, permission-checked, logged in `ai_tool_calls` and classified.

## Classes
- `read` — no side effects.
- `propose` — creates draft artifacts (insight, plan, brief, fact proposal). Safe; always allowed within scope.
- `generate_external` — calls a paid external generator (Higgsfield, render). Subject to cost limits.
- `execute` — **not available to agents.** Only the Execution Service performs provider writes.

## Read tools
| Tool | Returns |
|---|---|
| `get_client` | identity, status, compact summary, readiness |
| `get_growth_profile(client_id, sections[])` | scoped profile view |
| `get_offers`, `get_personas`, `get_goals`, `get_budget_context`, `get_funnel`, `get_creative_context`, `get_brand_kit` | profile sections |
| `get_client_decisions` | effective strategic decisions |
| `search_client_knowledge` | knowledge excerpts with source refs |
| `list_campaigns`, `get_entity(entity_ref)` | entity state |
| `get_performance(entity_ref, period, breakdown, metrics[])` | normalized metrics with comparison periods |
| `get_pacing(client_id)` | spend vs budget plan |
| `list_signals`, `list_insights`, `list_plans`, `get_action_history(entity_ref)` | loop artifacts |
| `get_creative_performance(filters)` | performance by creative/angle/hook |
| `search_media_assets(client_id, query, kind, consent_required)` | assets + derivatives + consent |
| `get_learnings(client_id, category)` | validated learnings |
| `get_conversion_outcomes` (M4) | client conversion source data |
| `get_crm_relationship_context` (M4, via Auto CRM MCP) | contract scope, relationship notes summary |

## Propose tools
| Tool | Creates |
|---|---|
| `create_insight` | insight (structured output) |
| `propose_plan(actions[])` | plan + plan_actions; Policy Engine runs on insert |
| `create_brief` | creative brief |
| `create_task` | task for a human |
| `propose_client_fact` | fact with status `inferred`/`needs_review` |
| `propose_learning` | learning with status `proposed` |
| `create_report_draft` | report draft |

## Generate tools (Creative Producer only)
`generate_image`, `generate_video`, `animate_image` (Higgsfield wrappers) · `render_composition(skill, props)` (Remotion) · `edit_video(ops)` (ffmpeg cut/concat/caption burn) · `remove_background`. Each records cost and stores outputs in the Media Library.

## Human-only operations
`confirm_client_fact`, `approve_plan`, `reject_plan`, change autopilot envelope, toggle kill switch, manage connections.

## Action catalog (for plan_actions, executed by the Execution Service)
| Action type | Phase | Rollback |
|---|---|---|
| `meta.ad.set_status` (pause/activate) | M2 | inverse status |
| `meta.adset.set_status` / `meta.campaign.set_status` | M2 | inverse status |
| `meta.adset.update_budget` / `meta.campaign.update_budget` | M2 | previous budget |
| `meta.budget.reallocate` (composite) | M2 | previous budgets |
| `meta.adset.update_bid` | M2 | previous bid |
| `meta.adset.duplicate` | M2 | pause duplicate |
| `meta.ad.create_from_creative` | M3 | pause ad |
| `meta.adset.update_schedule` | M2 | previous schedule |
| `meta.campaign.create` (full structure) | M4+ | pause campaign |
| `meta.adset.update_targeting` | M4+ | previous targeting |

Never in catalog: delete entities, billing/payment, account settings, pixel/dataset configuration, user/permission management.
