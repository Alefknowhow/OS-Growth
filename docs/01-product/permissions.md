# Permissions

Design for multi-tenant access from day one.

Planned roles:
- owner
- admin
- manager
- media_buyer
- creative
- client

Every tenant-owned record must be scoped by organization_id. Client-specific operational records should also carry client_id.

AI actions are subject to the same authorization model as humans plus AI-specific policies and approval requirements.
