# Media Library

The client's raw material, stored once and reused by every creative workflow.

## Asset kinds
Logos (variants), brand kit (colors, fonts, templates), photos, videos (testimonials, procedures, behind-the-scenes, talking head), audio (voice, music with license), documents (brand book, price lists), previous ads, AI-generated assets.

## Ingestion
- Internal upload (drag-and-drop, resumable for large videos).
- Client Portal upload (with a guided "what to record" checklist generated from the creative plan).
- Import from shared links (e.g. Google Drive folder) — copied into our storage, never referenced only by external URL.
- Generated assets (Higgsfield, renders) saved back with `source = generated`.
- Imported ad creatives from Meta (for performance history).

## Processing pipeline (worker)
1. Validation: type, size, checksum dedup, malware scan.
2. Technical metadata: dimensions, duration, fps, orientation, codec, audio presence.
3. Derivatives: thumbnail, web proxy, waveform; background-removed logo/product cutouts when requested.
4. Video understanding: transcript with word timings, scene/shot detection, best-moment candidates (hooks), on-screen people.
5. AI description and tags (subject, setting, emotion, product/service, quality issues).
6. Quality score (resolution, lighting, audio, shake).
7. Rights status assignment.

## Rights & consent (mandatory before use)
- `usage_consent`: client authorized usage for ads.
- `likeness_consent`: people appearing authorized use of image/voice, and specifically whether **AI manipulation** (e.g. avatar, voice clone, generated motion) is allowed.
- Expiry, restrictions (e.g. "not for paid ads", "only organic").
Creative skills must refuse assets without the required consent.

## Organization & search
Collections per client, tags, brand kit, favorites. Search by tag, text, transcript phrase and (later) semantic similarity. Each asset shows which creatives used it and how those performed.

## Storage
Supabase Storage, private buckets, path `org/{organization_id}/client/{client_id}/assets/{asset_id}/…`. Signed URLs for viewing; providers (Higgsfield, renderers) receive short-lived signed URLs only.
