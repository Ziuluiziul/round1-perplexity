# Brief — Checklist Agent API presets (Sonar → Agent) · set/2026

**Pacote:** Brief padrão (R$ 50) — docs-only  
**Fontes:**  
- https://docs.perplexity.ai/docs/agent-api/migrate-from-sonar/overview.md  
- https://docs.perplexity.ai/docs/getting-started/pricing.md  

## Pergunta
Como migrar Sonar Chat Completions para Agent API e escolher preset sem estourar custo?

## Resumo acionável
1. Sonar Chat Completions **é** Agent API; Sonar suportado até **27 set 2026**. Novos projetos → Agent.  
2. Mapa oficial: Sonar→`fast` · Sonar Pro→`low` · Reasoning Pro→`medium` · Deep Research→`high` · SOTA→`xhigh`.  
3. Agent = `input` + `output` tipado (não `messages`/`choices`).  
4. Tools cobram separado: `web_search` 0,0025 · `fetch_url` 0,0005 · people/finance 0,005 · sandbox 0,03/sessão.  
5. Leia `usage.cost.total_cost` quando a resposta trouxer.

## Checklist de migração
- [ ] Trocar cliente para `responses.create` / `POST /v1/agent`  
- [ ] Mapear modelo Sonar → preset (tabela acima)  
- [ ] Validar streaming/`store` se usava async Sonar  
- [ ] Contar tool invocations no orçamento  
- [ ] Smoke com `preset=fast` antes de `high`/`xhigh`  
- [ ] Documentar no README: chat consumer ≠ créditos API  

## Exercício (entregável)
Orçar 100 jobs/dia: 80×`fast` + 20×`low` com 1 `web_search` cada — anotar fórmula (tokens estimados + tools) e citar Pricing.

## PIX
R$50 EMV `_2` (tag54=50) nas Pages/curso. Após PIX: comprovante.
