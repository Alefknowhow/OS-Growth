# Event Architecture

Use Inngest for asynchronous workflows and event-driven processing.

Initial event vocabulary:
- metrics.synced
- campaign.performance_changed
- creative.approved
- creative.rejected
- experiment.completed
- client.created
- report.requested
- task.completed
- crm.sale.created
- crm.meeting.processed
- crm.communication.created

Events should be versioned, idempotent where applicable, tenant-scoped and observable.
