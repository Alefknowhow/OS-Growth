# Motion Graphics (Remotion)

Programmatic video composition for captions, logo animation, end cards, offer animations and assembling edits.

## Why Remotion
Videos are React components rendered to MP4/PNG with props, so templates are versioned code, parameterised by brief + brand kit, testable and reproducible. Claude is strong at authoring and adapting these compositions.

## Usage model
- **Production:** each creative skill ships versioned compositions (`skills/creative/<slug>/compositions`). The Producer passes props (copy, asset URLs, colors, fonts, timings, caption words with timestamps); it does not write new code per job.
- **Authoring:** new or custom compositions are drafted by Claude (Claude Code or an M3 Managed Agents spike), rendered in a sandbox, reviewed, then committed as a new skill version.

## Rendering
- Not on Vercel functions. Use the media/render worker container (Chromium + ffmpeg) or Remotion's serverless rendering on a cloud provider; decide in the M3 spike on cost/latency.
- Render jobs are queued by Inngest; outputs written to Supabase Storage; each render step recorded in `creative_job_steps` with duration/cost.

## Licensing
Check Remotion's license terms for company use before production (a company license may be required depending on team size).

## Supporting tooling
ffmpeg for cuts, concatenation, audio normalization, transcoding; transcript word timings (from the media pipeline) for animated captions.
