# Client Intelligence Center

One hub per client. Opening a client shows everything needed to understand, operate and report on the account.

## Header (always visible)
- Client name, status, assigned team.
- Health score (goal attainment, pacing, open critical signals, tracking health).
- Spend month-to-date vs budget, pacing indicator.
- Primary goal progress (e.g. CPL vs target, leads vs target).
- Readiness state (`setup_required` … `operational`).
- Autopilot status (off / approval-only / envelope active) and client kill switch.
- Command bar scoped to this client (voice or text).

## Tabs

### Overview
KPI cards vs targets, trend charts, top signals and insights, pending plans, recently executed actions, creative pipeline status, next scheduled report.

### Campaigns
Panel with all campaigns across connected accounts: status, objective, budget, spend, primary KPI, trend sparkline, open signals, pending plans. Filters by status, objective, period, channel.

#### Campaign detail
- KPI summary and trends vs target and previous period.
- Breakdown table: ad sets → ads (spend, results, CPA/CPL, CTR, CPM, frequency, ROAS when available).
- **Ad previews**: rendered placements for each ad (Meta Ad Previews) plus the source creative from the Creative Library.
- Pacing and budget history.
- Change history: actions by Growth OS (with plan link) and external edits detected by sync.
- Signals and insights for this campaign.
- Plans: pending, executed, rolled back.
- Quick actions: pause/activate, adjust budget, duplicate, swap creative, request creatives, generate report. Each quick action creates a Plan that goes through the Policy Engine (it may execute instantly if inside the autopilot envelope).
- Quick links: open in Ads Manager, related media assets, related creatives, related experiments.

### Creatives
Creative Library for the client with performance per creative (spend, CTR, hook rate, CPA), lifecycle status, variants and learnings (winning/losing angles). Button: "Request creatives".

### Media Library
Raw material: logos, photos, videos, brand kit. Upload, search (tags, transcript), rights/consent status. See `media-library.md`.

### Plans & Actions
All plans for the client with policy decision, approval, execution, outcome and rollback.

### Experiments
Running and concluded tests, results and learnings.

### Reports
Generated reports, schedule, publish status in the portal, "generate now".

### Profile
Growth Profile (business, offers, ICP, positioning, goals, budget, funnel, competitors, creative context), with fact status (confirmed / inferred / needs review) and a review queue for AI-proposed facts.

### Knowledge
Documents and references, plus summaries derived from Auto CRM meetings (as reviewable facts).

### Activity
Audit trail: commands, AI runs, approvals, executions, logins, portal activity.

### Settings
Connections (ad accounts, pixels, pages), autopilot envelope, approval rules, report schedule, portal users, Auto CRM link.
