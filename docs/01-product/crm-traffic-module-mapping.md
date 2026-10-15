# Mapeamento — Fluxo operacional × Althos CRM (módulo de tráfego)

Decisão: ADR-007. O CRM é o sistema (dados, aprovações, métricas). O Claude Code faz a execução via MCP.

## Divisão de papéis

| Camada | Responsável |
|---|---|
| Painel de clientes, central do cliente, métricas diárias, biblioteca/aprovação, portal do cliente | **CRM** |
| Estratégia, copies, roteiros, prompts e geração de criativos, montagem e publicação de campanha, review semanal | **Claude Code** (skills + MCP do CRM + MCP/API Meta, Google, geração de imagem/vídeo) |
| Registro do que a IA fez | `agent_audit_log` (já existe) |
| Ações que precisam de OK humano | `agent_pending_approvals` + `ads_policies` (já existem) |

## O que já existe no banco (Supabase `althos-crm-saopaulo`)

| Necessidade do fluxo | Tabela existente | Situação |
|---|---|---|
| Cliente | `contatos` (+ `projetos`, `client_portal_memberships`) | ok |
| Contas de anúncio | `ad_accounts`, `google_ads_connections`, `google_analytics_connections` | Meta ok; Google read-only ainda vazio |
| Campanhas + métricas diárias | `campaigns`, `campaign_metrics_daily` | ok (39 campanhas / 377 linhas) |
| Saúde de tracking/conta | `traffic_account_checks`, `capi_event_log`, `integration_health_checks` | criado, sem uso |
| Plano de mídia / estrutura | `media_plans`, `media_plan_items` (Campanha → Conjunto → Anúncio) | base do Campaign Builder |
| Aprovação de criativos pelo cliente | `campaign_creatives` (link público), `library_assets`, `library_asset_comments` | criado, sem uso |
| Tarefas / etapas | `tasks`, `projetos`, `projeto_grupos`, `project_templates` | ok |
| Conhecimento / estratégia | `library_items`, `org_documents` | parcial |
| Acesso de agentes via MCP | `agent_tokens`, `agent_audit_log` | ok (5k chamadas) |
| Guardrails de escrita em Ads | `ads_policies`, `agent_pending_approvals` | criado; escrita Meta bloqueada até App Review `ads_management` |
| Insights de IA | `ai_insights`, `dashboard_insights` | parcial |

## Lacunas (ajustar o que existe, não reconstruir)

1. **Growth Profile do cliente**: falta lugar estruturado para oferta, ICP, posicionamento, metas/KPIs e claims proibidos. Opção leve: tabela `client_profiles` (1:1 com contato, seções em jsonb com status confirmado/inferido) ou usar `library_items` por cliente.
2. **Estado do Campaign Builder**: adicionar em `media_plans` um campo `stage` (briefing → estrategia → copies → briefing_criativo → producao → aprovacao → qa → publicado → monitoramento → fechado) + `copies`/`creative_prompts` em `media_plan_items.config`.
3. **Slot de criativo**: ligar `media_plan_items` (nível anúncio) a `library_assets`/`campaign_creatives`, permitindo slot vazio até o criativo existir.
4. **Regras de otimização e alertas**: `ads_policies` cobre limites de escrita; falta `optimization_rules` (gatilho por cliente) e uso de `ai_insights` como feed de alertas.
5. **Review semanal**: guardar como documento por cliente/semana (decisões, MGC, próximos passos) — pode ser `org_documents` ou tabela `weekly_reviews`.
6. **Escrita na Meta**: depende do App Review. Até lá o Claude cria a campanha via MCP da Meta (fora do CRM) e registra os IDs em `campaigns`.

## Fluxo de execução com Claude Code

1. `/nova-campanha cliente=X` → lê perfil + histórico + métricas pelo MCP do CRM.
2. Gera estrutura e grava em `media_plans` (stage = estrategia).
3. Gera copies e prompts → `media_plan_items.config`; cria slots de criativo.
4. Gera imagens/vídeos (Nano Banana / Higgsfield via API) → sobe em `library_assets` → link de aprovação para o cliente (`campaign_creatives`).
5. Aprovado → checklist de QA → cria na Meta **pausada** → grava IDs → stage = publicado.
6. Tarefa agendada diária: lê `campaign_metrics_daily`, aplica regras, grava alertas.
7. Toda segunda: gera review semanal e tarefas.

## Skills a criar
- `nova-campanha` (etapas 1–5, reutiliza `campaign-structure`, `skill-texto`, `video-use`)
- `subir-campanha` (QA + publicação)
- `monitor-diario` (regras de alerta)
- `review-semanal` (SOP 05 do althos-growth)
