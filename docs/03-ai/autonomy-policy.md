# Autonomy & Policy

## Levels
0. Observe/read only
1. Recommend
2. Prepare action; human approval required
3. Execute pre-authorized bounded rules
4. Controlled autonomy inside explicit limits

MVP target: levels 1–2.

Before level 3, implement:
- Policy Engine
- Approval Engine
- role/permission checks
- spend/change limits
- idempotency
- rollback/recovery strategy where possible
- complete audit trail

No agent receives unrestricted ad-account write access.
