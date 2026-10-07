# Creative Skills

A creative skill is a versioned, pre-programmed production recipe for one format. When a creative is requested, the system already knows the format, required inputs, tools to use, output specs and QA checklist — and executes it.

## Location & anatomy
```
skills/creative/<slug>/
  SKILL.md          # purpose, when to use, step-by-step procedure, quality bar
  spec.json         # input schema, asset requirements, output specs, tools_allowed, cost ceiling
  qa.json           # QA checklist (machine-checkable + model-checked items)
  compositions/     # Remotion compositions (React) used by the skill, if any
  examples/         # reference outputs and briefs used in evals
```
`SKILL.md` follows the Agent Skills convention (frontmatter `name`, `description`) so the same skill can be loaded by Claude (Agent Skills or the Producer's system prompt) and by Claude Code/Claude apps when the operator works interactively.

Skills are registered in `creative_skills` on deploy (slug + version). Jobs record the skill version they ran.

## Execution (Creative Producer)
1. Validate brief against `spec.json` input schema.
2. Select assets via `search_media_assets` honoring consent requirements; if missing, stop and create a task ("ask client for X") instead of degrading quality.
3. Content Agent produces copy/script/captions within claims rules.
4. Run generation/edit steps with allowed tools only (Higgsfield wrappers, `render_composition`, `edit_video`).
5. Render each variant to output specs.
6. Creative QA → pass, or one automatic revision, then human review.

## Initial catalog
| Slug | Output | Inputs/assets | Tools |
|---|---|---|---|
| `static-offer` | feed 1:1 and 4:5 images | logo, product/service photo, offer, headline | Higgsfield (image/background), Remotion still render |
| `stories-static` | 9:16 image | same as above | Remotion still render |
| `carousel` | 3–10 cards 1:1/4:5 | offer, key points, photos | Remotion stills (or an external carousel tool via MCP) |
| `ugc-cut-reels` | 9:16 video 15–45s | client talking-head/testimonial video | transcript → hook selection, ffmpeg cuts, Remotion captions, logo, end card |
| `motion-offer` | 9:16 / 4:5 animated promo 8–20s | logo, photos, offer copy, brand kit | Remotion composition |
| `ai-broll-reels` | 9:16 video | client footage + script | Higgsfield video b-roll, Remotion composition |
| `photo-to-video` | short animated clip from a still | product/space photo | Higgsfield image-to-video, Remotion overlay |
| `testimonial-quote` | static/video quote card | testimonial transcript or text | Remotion |

## Output specs (defaults)
9:16 1080×1920, 4:5 1080×1350, 1:1 1080×1080; H.264/AAC MP4; safe zones for UI overlays; captions burned in; max text density per frame; logo placement rules from the brand kit; durations per placement.

## QA checklist (examples)
Dimensions/duration/codec; safe zones; logo present and correct variant; brand colors; legibility (contrast, size); no prohibited claims (text + transcript); spelling (pt-BR); consent for every asset with people; no AI manipulation of people without `ai_allowed`; CTA present; first-3-seconds hook present for video.

## Adding a skill
New skills (or changes) require example briefs + expected outputs for evals and a review PR. The operator can ask Claude to draft a new skill from a reference creative; it becomes active only after review.
