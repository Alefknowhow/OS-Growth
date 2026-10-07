# Client Onboarding

The onboarding creates the minimum trustworthy context required for Growth OS humans and agents to operate a client account.

## Entry points
- **Auto CRM `deal.won`** (default): client is created automatically with prefilled business data, contracted scope, media budget and the Auto CRM account link. Prefilled data arrives as `inferred` facts with `source_type = auto_crm` until reviewed.
- Manual creation in Growth OS (for clients not managed in Auto CRM).

## Minimum profile for M1
Required to reach `ready_for_analysis`: business summary, primary offer, primary goal with KPI target, monthly media budget, at least one connected ad account, prohibited claims (may be "none").
Required additionally for `ready_for_creative`: one persona/segment, positioning/value proposition, tone of voice, logo + brand colors, at least one usable media asset with consent.
Everything else is progressive.

## UX principle
Do not present one giant form. Use a resumable guided setup with visible completeness and the ability to mark information as unknown.

## Step 1 — Business
Collect name, website, niche, geography, business model, sales model, seasonality and a concise business description.

## Step 2 — Offers
Create one or more offers/products/services. Mark the primary acquisition offer and its commercial constraints.

## Step 3 — Audience
Define ICP/segments and optional personas. Capture pains, desires, objections, triggers and disqualifiers.

## Step 4 — Positioning
Value proposition, differentiators, proof, approved/prohibited claims, tone and brand language.

## Step 5 — Objectives
Create time-bounded goals and KPI targets. Separate business outcomes from media diagnostic metrics.

## Step 6 — Budget & Constraints
Media budget, test budget, channel allocation, operational capacity and approval/compliance constraints.

## Step 7 — Funnel
Map landing destinations, lead capture, qualification, sales stages, CRM mapping and revenue/conversion feedback.

## Step 8 — Creative Context & Media
Brand kit (logos, colors, fonts), existing assets, pillars, formats, spokesperson availability, brand rules and known creative learnings. Upload raw media to the Media Library and record usage/likeness consent (see `media-library.md`). Generate a "what to record" checklist for the client.

## Step 9 — Connections
Link the client's Meta ad accounts (shared to the agency Business Manager), pixel/datasets and pages. Later Google Ads, GA4 and client conversion sources. The Auto CRM link is created automatically on `deal.won`. Connection health is shown separately from profile completeness.

## Step 10 — Knowledge
Upload/link relevant documents and references.

## Step 11 — Policies & Portal
Approval rules and initial autopilot envelope (conservative default), report schedule, portal users and whether creatives need client approval.

## Step 12 — Review
Show a structured summary with:
- confirmed information
- missing critical information
- inferred information awaiting review
- connection status
- readiness for AI workflows

## Completeness
Completeness should be workflow-aware, not cosmetic. For example, Creative Strategist readiness requires an offer, audience context, positioning and goal; Performance Analyst readiness requires goals/KPIs and connected performance data.

Suggested readiness states:
- setup_required
- partially_ready
- ready_for_analysis
- ready_for_creative
- operational

## AI-assisted onboarding
AI (Onboarding/Context agent) may extract suggested profile facts from approved documents, the website, Auto CRM meeting summaries and proposals. Suggestions must be reviewable and retain provenance before becoming confirmed context.
