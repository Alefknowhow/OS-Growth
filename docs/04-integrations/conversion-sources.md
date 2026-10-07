# Client Conversion Sources (M4)

To optimise for real business outcomes, Growth OS needs the client's lead quality and sales — not only platform-reported conversions.

## Sources
- Meta lead forms (leads + later status updates).
- Pixel / Conversions API datasets (client site events).
- The client's own CRM or spreadsheet (lead status, sale, value) via webhook, scheduled CSV or API.
- Auto CRM, only if it is used to manage the client's leads.

## Data
`conversion_events`: client_id, source, external_lead_id (hashed identifiers when possible), attribution ids (fbclid, ad id, campaign id, UTM), stage (`lead|qualified|opportunity|sale`), value, currency, occurred_at.
Minimal PII; hashed where matching needs it.

## Uses
True CPL/CPA/ROAS by campaign/ad/creative, quality-weighted optimisation, offline conversion upload back to Meta (Conversions API) as a governed action.
