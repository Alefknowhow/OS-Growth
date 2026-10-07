# Client Portal

Restricted experience for each client (M4). Same app, separate audience, strict scope.

## Capabilities
- **Dashboard:** simplified KPIs vs goals, spend vs budget, trends, top creatives (approved data only, no internal signals).
- **Reports:** published reports (web + PDF), history.
- **Creative approvals:** review creatives sent for client approval; approve / request changes with comments and annotations.
- **Media upload:** send logos, photos, videos; see the "what to record" checklist.
- **Requests:** ask for new campaigns/creatives; becomes a request in the client's Intelligence Center (not an automatic action).
- **Notifications:** email/WhatsApp-style notifications for reports and pending approvals (via Auto CRM or direct, decided in M4).

## Never exposed
Internal strategy notes, signals/insights drafts, plans, AI reasoning artifacts, other clients, credentials, operational controls, costs/margins of the agency.

## Access model
Portal users are separate principals (`client_portal_users`) bound to exactly one client. Every portal query is scoped by `client_id` server-side and by RLS. See `permissions.md`.
