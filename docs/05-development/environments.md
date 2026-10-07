# Environments

Planned environments:
- local
- preview
- production

Each environment uses isolated credentials/configuration. Production secrets must never be copied into documentation or committed files.

Growth OS should use its own Vercel project and Supabase project, separate from the CRM, while integrations use scoped credentials.

## Per-environment configuration (names only, values in the secret store)
Supabase (URL, anon key, service role), Inngest keys, Upstash Redis, Claude API key, Meta app id/secret and system user token reference, Higgsfield MCP credentials, Auto CRM webhook secrets (in/out) and MCP OAuth client, speech provider key, media worker endpoint/credentials, storage buckets.
Preview environments use a Meta test/sandbox setup and Auto CRM staging; production credentials are never available to preview deployments.
