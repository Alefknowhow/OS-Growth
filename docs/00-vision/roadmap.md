# Roadmap

Sequencing principle: read and understand before writing; govern before automating; automate the highest-hour tasks first.

## M0 — Foundation (current)
Architecture, data models, AI/autonomy model, integrations and workflow. Exit: this documentation set merged.

## M1 — Intelligence (read-only)
- Supabase project, auth, organizations, memberships, client assignments, RLS.
- Clients + minimum Growth Profile + onboarding (manual and Auto CRM `deal.won` prefill stub).
- Meta connection (agency Business Manager system user), entity + insights sync, restatement window, ad previews.
- Performance data model, deterministic signal detectors.
- Performance Analyst → insights; daily digest.
- Client Intelligence Center v1 (overview, campaigns panel, campaign detail with ad previews).
- Media Library v1 (upload, storage, thumbnails).
- Growth OS MCP server (read-only tools) → operator can query by voice through the Claude app.

## M2 — Plans & Controlled Execution
- Plan / action / approval model, Policy Engine, Approval Inbox (web + mobile).
- Media Buyer agent drafts plans; Execution Service for Meta (status, budget, bid, duplicate, create ad from approved creative).
- Pre-execution drift check, post-execution verification, rollback, outcome evaluation.
- Autopilot envelopes per client (conservative defaults), kill switches.
- Native voice commands in the Command Center (push-to-talk, voice approvals with read-back).
- Reports v1 (internal).

## M3 — Creative Automation
- Media processing pipeline (transcripts, scenes, tagging, rights).
- Brand kits, creative skills registry, Creative Producer + Creative QA agents.
- Higgsfield MCP adapter, Remotion render worker.
- Creative Library lifecycle, internal approval, publish via Plan, creative performance and learnings.

## M4 — Client Portal, Reporting & Auto CRM
- Client Portal (metrics, reports, creative approvals, media upload, requests).
- Scheduled reports published to portal.
- Full Auto CRM integration (events both ways + MCP both ways).
- Conversion sources (client CRM / offline conversions) for revenue feedback.

## M5 — Scale & Expansion
- Wider autonomy based on measured track record; experiments engine.
- Google Ads, GA4.
- Cross-client learnings (anonymized, tenant-safe).
- Additional channels for commands (e.g. messaging voice notes).
