# Permissions

Multi-tenant from day one. Every tenant record carries `organization_id`; client operational records also carry `client_id`.

## Principals
- **Members** (agency staff): `owner`, `admin`, `manager`, `media_buyer`, `creative`, `analyst`.
- **Client portal users**: bound to one client; separate table and session claims.
- **Agents**: service principals (`agent:<name>`) with a capability subset. When an agent acts because of a user command, it acts *on behalf of* that user and is limited by the intersection of both permission sets.
- **System**: workers (sync, render) with narrowly scoped service roles.

## Client assignments
Members access clients through `client_assignments` (owner/admin see all). A media buyer only sees and operates assigned clients.

## Capability matrix (initial)

| Capability | owner | admin | manager | media_buyer | creative | analyst | client | agent |
|---|---|---|---|---|---|---|---|---|
| View client data | all | all | assigned | assigned | assigned | assigned | own (portal view) | scoped run |
| Propose plan | ✓ | ✓ | ✓ | ✓ | creative plans | – | requests only | ✓ |
| Approve plan up to risk | critical | high | high | medium | – | – | – | via policy only |
| Configure autopilot envelope | ✓ | ✓ | – | – | – | – | – | – |
| Kill switch | ✓ | ✓ | ✓ | assigned | – | – | – | – |
| Manage connections | ✓ | ✓ | – | – | – | – | – | – |
| Request/approve creatives (internal) | ✓ | ✓ | ✓ | ✓ | ✓ | – | client approval | request only |
| Publish reports to portal | ✓ | ✓ | ✓ | ✓ | – | ✓ | – | via policy |
| Manage portal users | ✓ | ✓ | ✓ | – | – | – | – | – |

## Rules
- Policy and permissions are checked server-side for every tool call and execution, regardless of channel (UI, voice, MCP, API).
- `organization_id` is derived from the session, never trusted from input.
- An approver cannot approve plans above their risk ceiling; critical plans may require two approvers (configurable).
