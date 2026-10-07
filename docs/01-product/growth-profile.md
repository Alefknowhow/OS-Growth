# Client / Growth Profile

The Growth Profile is the canonical operational context used by humans and AI agents to understand a client before analyzing or recommending actions.

It is not a replacement for CRM contact/account data. It stores growth-specific facts, constraints, goals and approved strategic context.

## Design goals
- Structured enough for deterministic queries and agent tools.
- Flexible enough to evolve without turning the client table into a giant document.
- Evidence-linked: important facts can reference their source.
- Version-aware: strategic changes must not silently rewrite historical context.
- Human-reviewable: AI-derived context is distinguishable from confirmed facts.

## Profile sections

### 1. Business
- legal/display name
- website
- industry / niche
- business model
- geography / service area
- business summary
- seasonality
- sales model
- average sales cycle

### 2. Products & Services
Each offer can have:
- name
- category
- description
- price / price range
- recurring vs one-time
- margin when available
- priority
- landing page
- availability / capacity constraints
- differentiators
- conditions / guarantees
- status

### 3. ICP & Personas
- target segments
- demographic/firmographic attributes when relevant
- geography
- pains
- desires
- jobs-to-be-done
- objections
- buying triggers
- disqualifiers
- decision criteria
- awareness stage
- preferred channels

Personas are strategic models, not assumptions about individual leads.

### 4. Positioning & Messaging
- value proposition
- positioning statement
- key differentiators
- proof / authority
- approved claims
- prohibited claims
- tone of voice
- vocabulary to use
- vocabulary to avoid
- CTA patterns
- brand guidelines

### 5. Goals & KPIs
Goals are time-bounded and measurable:
- objective
- start/end date
- primary KPI
- target value
- secondary KPIs
- attribution expectation
- notes / constraints

Examples: lead volume, qualified leads, CAC, sales, revenue, ROAS.

### 6. Budget & Constraints
- monthly media budget
- budget by channel
- test budget
- maximum/minimum operational thresholds
- geographic restrictions
- schedule restrictions
- capacity constraints
- compliance/legal constraints
- approval requirements

### 7. Funnel & Conversion
- traffic destinations
- landing pages
- lead capture mechanism
- qualification process
- sales process
- conversion stages
- expected stage rates
- CRM mapping
- offline conversion availability
- attribution identifiers

### 8. Competitors & Market
For each relevant competitor:
- name
- URL
- positioning
- offer
- observed strengths
- observed weaknesses
- creative/message observations
- evidence/source
- last reviewed date

### 9. Creative Context
- existing creative pillars
- approved formats
- visual constraints
- winning/losing angles and hooks (derived from learnings; manual seeds allowed)
- proof assets
- spokesperson availability
- raw-material availability
- production constraints

### 10. Integrations
References to connected:
- Meta ad accounts, pixels/datasets, pages
- Google Ads accounts
- GA4 properties
- Auto CRM account (agency relationship)
- client conversion sources (client CRM, lead forms, offline conversions)
- websites / tracked domains

Credentials never live in the Growth Profile itself.

### 11. Knowledge Base
Documents and references such as:
- brand book
- product/service material
- presentations
- landing pages
- FAQs
- sales scripts
- previous reports
- research
- approved meeting/Auto CRM-derived insights

Raw media (logos, photos, videos) lives in the Media Library, not in the Knowledge Base.

### 12. Strategic Notes & Decisions
Store explicit decisions with date, author/source, rationale and status.

## Fact status
Important profile facts should support:
- confirmed
- inferred
- needs_review
- rejected
- outdated

AI may propose/infer context, but inferred facts must not silently become confirmed. Profile tables hold the current confirmed state; `client_facts` is the review and provenance layer that feeds them (see `docs/02-architecture/growth-profile-data-model.md`).

## Provenance
Where useful, a fact can include:
- source_type
- source_reference
- captured_at
- captured_by
- confidence
- reviewed_by
- reviewed_at

## Agent consumption
Agents should request purpose-specific context instead of injecting the entire profile into every prompt.

Examples:
- Performance Analyst: goals, KPIs, budget, funnel, constraints, campaign context.
- Creative Strategist: offer, ICP, positioning, creative history, goals and performance evidence.
- Content Agent: approved strategy, offer, persona, tone, claims, restrictions and requested format.
- Orchestrator: compact client summary plus the context required by the selected workflow.

## Initial onboarding
See `client-onboarding.md`.

The profile remains editable after onboarding and should surface stale/unknown critical fields over time.
