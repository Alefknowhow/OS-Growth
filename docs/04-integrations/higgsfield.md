# Higgsfield Integration (MCP)

Higgsfield provides generative image and video (e.g. image generation, image-to-video, video effects/motion presets) used by creative skills.

## Mechanism
Remote Higgsfield MCP server, consumed by Growth OS's MCP client in the media worker. Each allowed Higgsfield tool is wrapped as a typed Growth OS tool (`generate_image`, `generate_video`, `animate_image`, …) so calls are permission-checked, cost-tracked and audited (see `../02-architecture/mcp-architecture.md`).

## Spike (start of M3) — confirm before building
- Exact tool list, parameters and output formats exposed by the MCP server.
- Authentication method and how credentials are issued (agency account).
- Job model (sync vs async/polling), latency, concurrency limits.
- Cost/credit model per operation and how to read usage.
- Output URL lifetime and download method.
- Terms of use for commercial ads and for generating/animating real people.

## Rules
- Inputs: only assets with the required consent; providers receive short-lived signed URLs.
- Outputs are downloaded immediately into the Media Library (`source = generated`, `generated_by_job_id`) with prompt/params recorded.
- Never generate or animate a real person's likeness without `likeness_consent = ai_allowed`.
- Per-job and per-client monthly generation cost limits; exceeding requires approval.
- Higgsfield is behind the adapter interface so other generators can be swapped in per skill.
