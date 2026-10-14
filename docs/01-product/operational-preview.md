# Preview Operacional — Growth OS (Desktop)

> Status: rascunho descritivo para validar usabilidade **antes** da simulação gráfica.
> Base: docs deste repositório (M0) + método do repositório `althos-growth` (Sistema Althos de Aquisição, SOPs e templates OHC / Experiment / Evidence / Optimization / Scale / LEARN).

---

## 1. Leitura do repositório — onde o projeto está hoje

| Item | Situação |
|---|---|
| Fase | M0 — Foundation (só documentação, sem código) |
| Fronteira | CRM = contatos/vendas/receita. Growth OS = mídia, criativos, experimentos, performance, aprendizado |
| Stack planejada | Next.js + Supabase (Auth/Storage/Postgres) + Vercel + Inngest + Redis + OpenAI/Anthropic |
| Integrações | Meta primeiro (leitura), Google Ads / GA4 / CRM depois |
| IA | 4 agentes: Orchestrator, Performance Analyst, Creative Strategist, Content Agent |
| Autonomia | Níveis 0–4. MVP só 1–2 (recomendar e preparar ação com aprovação humana) |
| Método | Já existe no `althos-growth`: 7 fases (Diagnosticar → Fundamentar → Ofertar → Adquirir → Validar → Otimizar → Escalar), review semanal/mensal, cards |

**Lacunas que esta proposta cobre:**
1. Não existe descrição de telas nem de navegação.
2. O fluxo de "subir campanha" não está descrito (as docs só falam em escrita na Meta no M4).
3. O método Althos (cards, Validation Ladder, MGC) ainda não está ligado ao produto.
4. Biblioteca de aprovação com o cliente está prevista apenas como "Client Portal / later".

---

## 2. Mapa de navegação (desktop)

```
Sidebar fixa
├── Command Center            (todos os clientes ativos)
├── Clientes                  (grid de cards)
│   └── [Cliente] — Central do Cliente
│       ├── Visão geral
│       ├── Estratégia (Growth Profile)
│       ├── Campanhas  → Campaign Builder (fluxo linear)
│       ├── Criativos  → Biblioteca + Aprovações
│       ├── Analytics
│       ├── Experimentos & Aprendizados
│       ├── Otimização (semanal)
│       ├── Tarefas
│       └── Conexões & Acessos
├── Aprovações                (fila global: o que espera você)
├── Tarefas                   (kanban global)
├── Growth AI                 (chat + log de ações dos agentes)
└── Configurações             (regras de otimização, playbooks, usuários)
```

Layout padrão: sidebar esquerda (240px) · área principal · painel direito recolhível "Copiloto" (IA com contexto do cliente aberto).

---

## 3. Telas — descrição

### 3.1 Command Center
Pergunta que responde: **"Onde preciso colocar atenção hoje?"**

- **Faixa de KPIs agência**: investimento do mês, leads, CPL médio, contas em alerta, aprovações pendentes.
- **Grid de cards de clientes ativos** — cada card mostra:
  - nome, nicho, fase do método (ex.: `04 Adquirir`);
  - semáforo de saúde (verde / amarelo / vermelho) calculado pelas regras;
  - gasto do mês vs. orçamento (barra de pacing);
  - KPI principal vs. meta (ex.: CPL R$ 38 / meta R$ 45);
  - badges: `3 criativos aguardando cliente`, `1 campanha em rascunho`, `tracking quebrado`.
- **Feed de atenção** (coluna direita): alertas dos agentes ordenados por impacto ("Cliente X: frequência 4,2 no conjunto Y, CTR caiu 38% em 5 dias").
- Filtros: responsável, nicho, fase, saúde.

### 3.2 Central do Cliente (ao clicar no card)
Cabeçalho fixo: logo, nome, fase atual, saúde, meta do período, botões **[+ Nova campanha]** **[Abrir no Meta]** **[Abrir no Google]** **[Pedir análise à IA]**.

**Aba Visão geral**
- Resumo do mês (gasto, resultados, KPI vs meta), gráfico diário 30 dias.
- "Agora" (prioridades NOW do último review semanal).
- Últimos alertas, últimas mudanças feitas nas contas (change log), próximos passos.

**Aba Estratégia (Growth Profile)**
- As 12 seções do Growth Profile em blocos editáveis (negócio, ofertas, ICP, posicionamento, metas, orçamento, funil, concorrentes, contexto criativo, integrações, base de conhecimento, decisões).
- Cada fato com status `confirmado / inferido / desatualizado / revisar`.
- Indicador de prontidão: `pronto para análise`, `pronto para criativo`, `operacional`.
- Documentos: diagnóstico, OHCs (hipóteses de oferta), Mapa de Aquisição.

**Aba Campanhas**
- Lista de campanhas ativas/pausadas por plataforma com métricas do período.
- Rascunhos do Campaign Builder com etapa atual (ver seção 5).

**Aba Criativos — Biblioteca e Aprovações** (seção 4.3)

**Aba Analytics**
- Funil por camada (entrega → atenção → clique → lead → qualificado → venda) — mesmas camadas do método.
- Ranking de criativos (hook rate, hold rate, CTR, CPL, qualidade).
- Comparativo período vs período; quebra por campanha/conjunto/anúncio/posicionamento.
- Quando CRM conectado: CPL × custo por lead qualificado × CAC × ROAS.

**Aba Experimentos & Aprendizados**
- Experiment Cards (hipótese, variável, KPI, orçamento, prazo, status).
- Validation Ladder: ruído → sinal → evidência.
- Biblioteca LEARN do cliente (e, de forma anonimizada, do nicho).

**Aba Otimização** (seção 6)

**Aba Conexões & Acessos**
- Contas Meta / Google / GA4 / pixel / CRM com status de saúde (token válido, última sincronização, erros).
- Links diretos para os gerenciadores. Nunca guarda senhas: só referências e tokens OAuth no cofre.

---

## 4. Casos de uso (simulação de usabilidade)

Personas: **Gestor** (você), **Designer/Editor**, **Cliente**, **IA** (agentes).

### UC-01 — Começar o dia (5 min)
1. Gestor abre o Command Center.
2. Vê 2 cards vermelhos e 1 amarelo; o feed já explica o motivo de cada um.
3. Clica no alerta → abre a Central do Cliente direto na aba Analytics com o filtro do problema.
4. Aceita a recomendação da IA ("pausar anúncio 07, frequência alta") → vai para Aprovações → executa ou cria tarefa.

**Critério de sucesso:** do login à primeira decisão em < 2 minutos, sem abrir o Gerenciador de Anúncios.

### UC-02 — Onboarding de cliente novo
1. `+ Novo cliente` → assistente em 11 passos (retomável, aceita "não sei").
2. IA lê site/Instagram/documentos e sugere fatos marcados como `inferido`.
3. Gestor confirma; conecta Meta/Google (OAuth).
4. Sistema mostra prontidão e cria automaticamente as tarefas da Fase 01–02 (diagnóstico e fundação/tracking) a partir dos SOPs.

### UC-03 — Cliente envia material bruto
1. Cliente (link de portal ou upload interno) envia fotos/vídeos em **Brutos**.
2. Sistema organiza por data/tema; IA descreve cada arquivo (quem aparece, cenário, duração, qualidade) e sugere em quais roteiros ele serve.
3. Editor é notificado com uma tarefa.

### UC-04 — Aprovação de criativos
Pipeline: `Bruto → Em produção → Revisão interna → Aguardando cliente → Aprovado / Ajustes / Reprovado → Publicado`.
1. Editor sobe a peça final vinculada a um roteiro/briefing.
2. Gestor aprova internamente (checa claims proibidos — a IA destaca riscos de política do Meta e do conselho profissional).
3. Cliente recebe link, vê a peça com a copy, comenta **no frame / no timestamp** e clica Aprovar ou Pedir ajuste.
4. Aprovado → criativo fica disponível no Campaign Builder.

### UC-05 — Montar e subir campanha (fluxo linear, seção 5)

### UC-06 — Review semanal (seção 6)
Toda segunda a IA gera o rascunho do review (SOP 05); gestor revisa em 15 min, decide e o sistema cria as tarefas.

### UC-07 — Review mensal / relatório ao cliente
IA gera relatório a partir dos dados + aprendizados; gestor ajusta; envia link ao cliente.

### UC-08 — Incidente
Pixel parou, conta bloqueada, gasto acima de 130% do ritmo → alerta vermelho imediato + checklist do SOP 07 aberto como tarefa.

---

## 5. Campaign Builder — processo linear com início e fim

Princípio: **a campanha começa antes do criativo existir**. O criativo é um "slot" que é preenchido depois.

| # | Etapa | Quem faz | IA faz | Saída / gate |
|---|---|---|---|---|
| 1 | **Briefing** | Gestor escolhe cliente, oferta (OHC), objetivo, orçamento, prazo | Puxa Growth Profile e aprendizados anteriores | Briefing aprovado |
| 2 | **Estratégia** | Revisa | Propõe estrutura (campanhas, conjuntos, públicos, verba por conjunto, hipótese de teste — Experiment Card) | Estrutura aprovada |
| 3 | **Copies** | Edita / aprova | Gera headlines, textos primários, descrições, CTAs em variações por ângulo, respeitando claims e tom | Copies aprovadas |
| 4 | **Briefing de criativos** | Aprova | Gera roteiros, hooks e **prompts** prontos para imagem/vídeo (Nano Banana, Higgsfield etc.) + lista de brutos necessários | Slots de criativo criados (vazios) |
| 5 | **Produção** | Editor / geração por IA | Opcional: chama API de geração e salva o resultado na biblioteca | Peças anexadas aos slots |
| 6 | **Aprovação** | Interna + cliente (UC-04) | Checagem de política / specs (proporção, duração, texto em imagem) | Todos os slots aprovados |
| 7 | **QA pré-lançamento** | Gestor confere checklist | Valida UTMs, pixel/evento, nomenclatura, orçamento, datas, links (SOP 03) | Checklist 100% |
| 8 | **Publicação** | Gestor clica "Publicar" | Cria tudo na Meta/Google **pausado**, retorna IDs | Gestor ativa (ou agenda) |
| 9 | **Monitoramento 72h** | — | Vigia gasto, entrega, rejeições, erros | Alerta se algo foge da regra |
| 10 | **Fechamento** | Decide | Após janela do teste, preenche Evidence Card + LEARN | Campanha entra no ciclo semanal |

Regras do builder:
- Não dá para pular etapa; dá para voltar. Cada etapa tem dono e prazo, e aparece em Tarefas.
- Nomenclatura gerada automaticamente: `CLIENTE_PLATAFORMA_OBJETIVO_OFERTA_PUBLICO_AAAAMMDD`.
- Publicação sempre cria em status **pausado** (autonomia nível 2 — coerente com `autonomy-policy.md`).

---

## 6. Otimização semanal e regras

### 6.1 Regras diárias automáticas (alertas, não ações)
| Regra | Gatilho (padrão, ajustável por cliente) | Ação sugerida |
|---|---|---|
| Pacing | gasto acumulado > 115% ou < 80% do ritmo | ajustar orçamento |
| Fadiga | frequência > 3,5 (7d) **e** CTR −30% vs 7d anteriores | trocar criativo |
| Anúncio sem tração | gasto ≥ 2× CPL-meta sem conversão | pausar anúncio |
| CPL estourado | CPL 3d > 1,5× meta com ≥ 1,5× CPL-meta gasto | revisar conjunto |
| Hook fraco | hook rate < 25% (vídeo) após 1.000 impressões | novo gancho |
| Tracking | 0 eventos em 24h com gasto > 0 | incidente |
| Rejeição | anúncio reprovado | corrigir / recurso |
| Vencedor | CPL ≤ 0,7× meta com ≥ 10 conversões | candidato à escala (Scale Card) |

### 6.2 Ritual semanal (segunda)
1. IA consolida números, mudanças, incidentes, experimentos em andamento.
2. Classifica cada hipótese na Validation Ladder.
3. Aponta o **MGC** (maior gargalo controlável) por cliente.
4. Propõe decisões: manter / iterar / aprofundar / pausar / otimizar / escalar / reformular, cada uma com Optimization Card.
5. Gestor aprova em lote → sistema gera tarefas, novos briefings de criativo (volta ao builder etapa 3–4) e ações preparadas.
6. Uma mudança relevante por conjunto por vez; escala de orçamento em passos de até 20–30% a cada 72h.

### 6.3 Evolução de autonomia
- Agora: alertas + ações preparadas com 1 clique de aprovação.
- Depois (M4): regras pré-autorizadas executam sozinhas dentro de limites (ex.: pausar anúncio sem tração; nunca subir verba sem aprovação).

---

## 7. Construir tudo vs. usar Claude Code — recomendação

Recomendação: **sistema leve + Claude Code como "operador"**.

**O sistema (Growth OS) guarda o que precisa ser visual, compartilhado e persistente:**
- Command Center, Central do Cliente, Analytics (sync diário Meta/Google → Supabase).
- Biblioteca de criativos e aprovação com o cliente (Supabase Storage + link público).
- Registro de campanhas, cards do método, aprendizados, tarefas.

**O Claude Code faz o trabalho pesado de montagem, sem você construir UI para isso no início:**
- Conectado via MCP a: Supabase (lê Growth Profile, grava rascunhos), Meta Ads, Google Ads, Drive/Canva e APIs de geração (Nano Banana / Gemini Image, Higgsfield) via scripts.
- Skills que já existem na sua conta e encaixam no fluxo: `campaign-structure` (etapas 1–3), `video-use` (roteiros, etapa 4), `skill-texto` (copies). Faltaria criar: `subir-campanha` (etapas 7–8 com checklist de QA) e `review-semanal` (seção 6.2).
- Fluxo: você diz "monta campanha de harmonização para cliente X, R$ 3 mil" → Claude lê o perfil no Supabase → gera estrutura, copies, prompts → gera imagens via API → salva tudo na biblioteca como "aguardando aprovação" → após aprovação, cria na Meta pausado.
- Agendamento: a rotina semanal pode rodar como tarefa agendada que gera o rascunho do review toda segunda.

Assim o MVP do sistema fica em: **Command Center + Central do Cliente + Biblioteca/Aprovação + Analytics**. O Campaign Builder visual (seção 5) entra depois, quando o fluxo via Claude Code estiver validado — ele vira apenas a interface das mesmas etapas.

---

## 8. Próximos passos sugeridos
1. Validar este documento (fluxos e regras).
2. Simulação gráfica das telas desktop: Command Center, Central do Cliente, Biblioteca/Aprovação, Campaign Builder, Review semanal.
3. Modelar tabelas novas no Supabase: `creative_assets`, `approvals`, `campaign_drafts`, `optimization_rules`, `alerts`.
4. Criar as skills `subir-campanha` e `review-semanal` e testar com 1 cliente real.
