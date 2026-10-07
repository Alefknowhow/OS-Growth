# AI Memory

Memory is structured, evidence-linked and stored in tables — not in chat transcripts.

| Layer | Store | Lifetime | Written by |
|---|---|---|---|
| 1. Raw evidence | raw_ingestion_batches, metrics, entity_changes, media, documents | retention policy | integrations |
| 2. Derived observations | signals, insights | time-bound (expires_at) | detectors, agents |
| 3. Outcomes & experiments | action_outcomes, experiment_results | permanent | evaluation workflows |
| 4. Learnings | learnings (client / vertical / organization) | until deprecated | agents propose, humans or rules validate |
| 5. Stable client knowledge | profile tables + confirmed client_facts + decisions | until changed | humans (confirm) |
| 6. Operator preferences | memories (e.g. "always keep remarketing ≥ R$30/day for client X") | until removed | operator, or agent-proposed + confirmed |

## Rules
- Do not convert model statements into durable memory automatically. Promotion to layers 4–6 requires validation (human or a deterministic rule such as "experiment concluded with significance").
- Every memory item has provenance (source refs), timestamps and status.
- Cross-client learnings (organization/vertical scope) never include client-identifying details and are derived only from aggregated outcomes.
- Agents retrieve memory through tools (`get_learnings`, `get_client_decisions`, preferences inside context packs), never by full dumps.

## memories table
id, organization_id, client_id nullable, kind (`preference|constraint|note`), statement, source_type, source_ref, status (`proposed|active|archived`), created_by, confirmed_by, created_at.
