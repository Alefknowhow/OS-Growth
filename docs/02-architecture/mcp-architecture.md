# MCP Architecture

Model Context Protocol (MCP) is used where an **agent** needs on-demand access to another system. It does not replace events/webhooks for system-to-system state sync.

| Need | Use |
|---|---|
| Reliable state change propagation (deal won, report published) | events / webhooks |
| Bulk or scheduled data sync | REST adapters |
| An agent querying or acting on another system during reasoning | MCP |
| The operator's Claude (desktop/mobile/voice) operating Growth OS | Growth OS MCP server |

## Growth OS as MCP client
Connected servers: **Higgsfield** (generative media), **Auto CRM** (relationship context), others later.

Default pattern: an MCP client inside Growth OS (TypeScript MCP SDK, in workflows or the media worker) wraps each allowed remote tool as a **typed Growth OS tool**. The agent sees the wrapped tool, so every call passes through permissions, policy, cost tracking and audit like any internal tool.

Optional pattern for low-risk, read-only prototyping: the Claude API MCP connector (remote server declared on the request, with a tool allowlist). Calls then go from Anthropic's infrastructure straight to the MCP server, so Growth OS cannot gate individual calls — not allowed for tools that write, spend credits beyond limits, or touch client data in other systems.

Rules:
- Allowlist tools per agent; unknown tools from a server are disabled until reviewed.
- MCP outputs are untrusted data (prompt-injection boundary). Generated media URLs are downloaded to our storage immediately.
- Credentials for remote servers live in Vault, referenced by `integration_connections`.

## Growth OS as MCP server
Exposed at `/mcp` (Streamable HTTP), OAuth 2.1 with per-user consent; the token maps to a Growth OS user, so all role, client-assignment and policy checks apply. Consumers: the operator's Claude apps (including voice), Auto CRM's AI, internal automations.

Tool groups (phased):
| Phase | Tools |
|---|---|
| M1 (read) | `list_clients`, `get_client_overview`, `get_campaign_performance`, `list_signals`, `list_insights`, `get_daily_digest`, `search_media_assets`, `get_report` |
| M2 (propose/approve) | `propose_plan`, `list_pending_approvals`, `get_plan`, `approve_plan`, `reject_plan`, `set_client_kill_switch` |
| M3 (creative) | `request_creatives`, `get_creative_job`, `review_creative` |
| M4 (CRM-facing) | `get_client_health`, `get_delivery_summary` (scoped tools for Auto CRM's agents) |

`approve_plan` through MCP follows the same risk rules as voice: high/critical plans require confirmation in the Growth OS UI (step-up), the tool returns a confirmation link instead of approving.

All MCP sessions and tool calls are recorded in `ai_tool_calls`/`audit_logs` with the channel `mcp`.
