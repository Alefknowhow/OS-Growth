# Creative Data Model

## Media
### media_assets
id, organization_id, client_id, kind (`logo|image|video|audio|document|font|template`), source (`client_upload|agency_upload|import|generated|ad_platform`), storage_path, mime, size_bytes, checksum, width, height, duration_ms, orientation, status (`uploaded|processing|ready|rejected|archived`), title, ai_description, tags[], transcript_ref, scenes (jsonb), people_present, quality_score, generated_by_job_id nullable, uploaded_by, created_at.

### media_asset_derivatives
asset_id, kind (`thumbnail|proxy|waveform|cutout|clip|frame`), storage_path, params (e.g. clip start/end), created_at.

### asset_consents
asset_id, usage_consent (bool), likeness_consent (`none|real_only|ai_allowed`), people (refs), restrictions, valid_until, evidence_ref (signed term/document), recorded_by.

### brand_kits
client_id, logos (asset refs by variant), colors, fonts (asset refs), typography rules, end-card template, lower-third template, music preferences, do/don't list, version.

## Production
### creative_skills (registry)
slug, version, format, placements, input_schema, asset_requirements, output_specs, tools_allowed, qa_checklist, status (`draft|active|deprecated`), source path in repo. See `../03-ai/creative-skills.md`.

### creative_briefs
id, client_id, origin (command/signal/experiment/client_request), hypothesis_id, objective, offer_id, persona_id, angle, hooks[], key_message, cta, format, skill_slug, variants_count, variant_dimension, constraints, asset_hints, status, ai_run_id.

### creative_jobs
id, brief_id, skill_slug, skill_version, requested_by (user/command/agent), status (`queued|selecting_assets|writing|generating|rendering|qa|awaiting_review|completed|failed|cancelled`), cost_estimate, cost_actual, cost_breakdown (jsonb by provider), started_at, finished_at, error.

### creative_job_steps
job_id, sequence, kind (`asset_selection|copy|generate_image|generate_video|edit|render|qa`), provider (`claude|higgsfield|remotion|ffmpeg|internal`), tool, input (jsonb), external_job_id, output_asset_ids[], status, cost, duration.

## Deliverables
### creatives
id, client_id, job_id, variant_group, variant_label, format, aspect_ratio, duration_ms, final_asset_id, copy (primary_text, headline, description, cta), qa_report (jsonb), status (`draft|internal_review|client_review|approved|published|retired|rejected`), angle, hook, learnings_refs.

### creative_reviews
creative_id, reviewer_type (`internal|client`), reviewer_id, decision (`approved|rejected|changes_requested`), comments, annotations (timestamped/positioned), created_at. Changes requested → revision job.

### creative_publications
creative_id, provider, ad_account_id, ad_creative_external_id, ad_external_id, plan_action_id, published_at, status.

`v_creative_performance` joins publications to metrics so performance rolls up by creative, angle and hook.
