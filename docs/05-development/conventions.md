# Development Conventions

- TypeScript strict mode.
- Prefer explicit domain types and schemas at boundaries.
- Validate all external inputs.
- Never expose service-role/API secrets to the client.
- Keep provider adapters behind internal interfaces.
- Tenant scope every query and mutation.
- Migrations are reviewed artifacts.
- Add tests for domain rules and consequential actions.
- Log external syncs and AI actions with correlation IDs.
- Avoid speculative abstractions; extract only after repeated use.
