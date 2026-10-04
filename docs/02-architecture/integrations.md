# Integration Architecture

External platforms are accessed through provider adapters.

## Principle
Agents do not directly embed provider-specific API calls. They call Growth OS tools/services; adapters translate those calls to Meta, Google, GA4 or CRM.

## Planned integrations
- Meta Marketing API
- Google Ads API
- GA4 Data API
- CRM API/Webhooks
- OpenAI
- Anthropic

## Data strategy
Synchronize advertising/analytics data into the Growth Data Layer. Avoid repeated live API reads during every agent reasoning cycle.
