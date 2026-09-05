# Ebook — Pesquisa & docs Perplexity (API Platform) · set/2026

**Pacote:** Mini-curso + ebook (docs-only) · Round 1 Receita Gamma  
**Data:** 2026-09-05 (America/Sao_Paulo)  
**Fonte única:** docs oficiais https://docs.perplexity.ai (WebFetch) — **sem API**, free parado.  
**PIX:** R$50 (ebook+curso) ou R$20 (módulo avulso) — EMV LUIZ nas Pages.

---

## Módulo 0 — Mapa do produto (5 min)

1. **Chat/app ≠ API.** Free / Pro (US$20/mês) / Max (US$200/mês) = assinatura consumer. Console `console.perplexity.ai` = créditos API separados.  
2. Superfícies API (docs overview): **Router**, **Agent**, **Search**, **Embeddings**, legado **Sonar** (suporte até **27 set 2026**).  
3. Docs index: https://docs.perplexity.ai/llms.txt  

**Exercício:** abrir Pricing e listar 3 linhas de preço que você usaria no seu produto.

---

## Módulo 1 — Sonar → Agent (brief R$20)

**Fato oficial:** Sonar Chat Completions agora é Agent API; Sonar permanece até 27/09/2026. Docs recomendam Agent para projetos novos e migrar o legado.

| Sonar | Preset Agent | Uso típico |
|-------|--------------|------------|
| Sonar | `fast` | lookup / resumo rápido |
| Sonar Pro | `low` | pesquisa leve multi-step |
| Sonar Reasoning Pro | `medium` | multi-hop / agregação |
| Sonar Deep Research | `high` | cobertura profunda |
| (SOTA) | `xhigh` | pesquisa mais pesada |

Chamada mínima Agent (docs migrate):

```python
from perplexity import Perplexity
client = Perplexity()
response = client.responses.create(preset="fast", input="…")
print(response.output_text)
```

**Checklist migração:** trocar endpoint/`messages`→`input`; ler `output` tipado; mapear modelo Sonar → preset; validar `usage.cost.total_cost` quando existir.

Fontes: https://docs.perplexity.ai/docs/agent-api/migrate-from-sonar/overview.md

---

## Módulo 2 — Precificação (brief R$50)

### Sonar (tokens + request fee)

| Modelo | In $/1M | Out $/1M | Request /1K Low·Med·High |
|--------|---------|----------|---------------------------|
| Sonar | 1 | 1 | 5 · 8 · 12 |
| Sonar Pro | 3 | 15 | 6 · 10 · 14 |
| Sonar Reasoning Pro | 2 | 8 | 6 · 10 · 14 |
| Sonar Deep Research | 2 | 8 | + citation $/1M 2 · search queries $/1K 5 · reasoning $/1M 3 |

**Pro Search** (Sonar Pro, `search_type`): `fast` 6/10/14 · `pro` 14/18/22 · `auto` varia — tokens iguais ao Sonar Pro.

### Search API
**US$ 5,00 / 1.000** `POST /search` bem-sucedidos (até 5 queries = 1 unidade). Sem token fee.

### Agent tools
| Tool | Preço |
|------|-------|
| `web_search` | US$ 0,0025 / invocação |
| `fetch_url` | US$ 0,0005 |
| `people_search` / `finance_search` | US$ 0,005 |
| `sandbox` | US$ 0,03 / sessão (~20 min billing) |

### Embeddings (padrão)
`pplx-embed-v1-0.6b` US$ 0,004 /1M · `pplx-embed-v1-4b` US$ 0,03 /1M  
Contextualized: 0,008 e 0,05 /1M.

Fonte: https://docs.perplexity.ai/docs/getting-started/pricing.md

**Exercício:** estimar custo de 1.000 lookups Sonar Low vs 1.000 Search API.

---

## Módulo 3 — Brief com citações (prática)

Template de entregável vendável (R$20 curto / R$50 padrão):

1. Pergunta do cliente  
2. Resumo acionável (5 bullets)  
3. Tabela/números **só** de docs oficiais + URL da página  
4. Limites (o que as docs **não** dizem)  
5. PIX + pedido de comprovante  

Amostra já publicada:  
- API≠chat: `briefs/ENTREGAVEL-brief-api-vs-chat-sonar-2026-09-05.md`  
- Citações: gist ENTREGAVEL-brief-ia-pesquisa-citacoes  

---

## Módulo 4 — Labor docs-only (quando free/API parados)

1. Não chamar console/API se Credits = $0 / free crash.  
2. Produzir com WebFetch em `docs.perplexity.ai` + `llms.txt`.  
3. Empacotar Pages + gist + PIX EMV (texto = fonte da verdade; R$20=`pix-r20` tag54=20; R$50=`_2` tag54=50).  
4. Reportar só **URL nova** ou **R$>0**.

---

## Encerramento — o que você leva

- Mapa chat vs API vs Sonar vs Agent vs Search  
- Tabela de preços oficiais (set/2026)  
- Roteiro de migração Sonar→Agent  
- Template de brief citado + pacote PIX  

**Compra:** Pages https://ziuluiziul.github.io/round1-perplexity/curso/ · após PIX, envie comprovante.

*Disclaimer: números sujeitos a mudança nas docs; sempre confira a página Pricing oficial.*
