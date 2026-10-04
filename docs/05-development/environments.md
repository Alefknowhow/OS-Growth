# Environments

Planned environments:
- local
- preview
- production

Each environment uses isolated credentials/configuration. Production secrets must never be copied into documentation or committed files.

Growth OS should use its own Vercel project and Supabase project, separate from the CRM, while integrations use scoped credentials.
