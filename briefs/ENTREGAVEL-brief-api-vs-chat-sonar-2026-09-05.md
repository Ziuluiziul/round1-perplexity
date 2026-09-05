# Brief — API vs chat Perplexity (Sonar / Agent / Search) · set/2026

**Pacote:** Brief curto (R$ 20) — amostra vendável Round 1  
**Data:** 2026-09-05 (America/Sao_Paulo) ~16:51  
**Público:** freelancers e makers que confundem assinatura consumer (Free/Pro/Max) com billing da API Platform.  
**Produção:** docs oficiais `docs.perplexity.ai` (WebFetch). API console **exhausted** ($0) — sem top-up; chat web free já crashou (`Aw, Snap!`) nesta missão → **docs-only**.  
**PIX amostra:** `pix_r20_copia_cola.txt` + `pix_r20_qr.png` (EMV com valor **20.00**; recebedor LUIZ — nunca inventar chave).

## Pergunta
Qual a diferença entre **chat/app** Perplexity e a **API Platform**, e quais são as taxas publicadas de Sonar, Search e ferramentas Agent?

## Resumo acionável
1. **Chat/app ≠ API.** Free / Pro (US$ 20/mês) / Max (US$ 200/mês) são assinatura consumer. Console em `console.perplexity.ai` é billing separado (créditos/API).  
2. **Sonar API** = tokens **+** taxa por request (contexto de busca Low/Medium/High).  
3. **Search API** = **US$ 5 / 1.000** requests bem-sucedidos (`POST /search`); sem cobrança por token.  
4. **Agent API** = tokens do modelo (sem markup nos third-party) **+** ferramentas por invocação (ex.: `web_search` US$ 0,0025).  
5. Nesta missão receita: **API free only**; Credits **US$ 0** → só chat web / docs; **sem top-up**.

## Tabela Sonar (oficial — docs pricing)

| Modelo | Input $/1M | Output $/1M | Request fee /1K (Low · Med · High) |
|--------|------------|-------------|-------------------------------------|
| Sonar | 1 | 1 | 5 · 8 · 12 |
| Sonar Pro | 3 | 15 | 6 · 10 · 14 |
| Sonar Reasoning Pro | 2 | 8 | 6 · 10 · 14 |
| Sonar Deep Research | 2 | 8 | (+ citation $/1M **2**, search queries $/1K **5**, reasoning $/1M **3**) |

Pro Search (`search_type: pro` em Sonar Pro): request fee **14 · 18 · 22** /1K (Low/Med/High); tokens iguais ao Sonar Pro.

## Search + Agent tools (oficial)

| Superfície | Preço publicado | Unidade |
|------------|-----------------|---------|
| Search API | **US$ 5,00** | por 1.000 requests OK |
| Agent `web_search` | **US$ 0,0025** | por invocação |
| Agent `fetch_url` | **US$ 0,0005** | por invocação |
| Agent `people_search` / `finance_search` | **US$ 0,005** | por invocação |
| Agent `sandbox` | **US$ 0,03** | por sessão (janela de billing ~20 min) |

Embeddings (padrão): `pplx-embed-v1-0.6b` **US$ 0,004** /1M tokens; `pplx-embed-v1-4b` **US$ 0,03** /1M.

## Quando usar o quê
| Objetivo | Caminho |
|----------|---------|
| Resposta citada no browser, sem código | Chat free / Pro / Max |
| Integrar busca citada no produto | Sonar ou Agent API (créditos) |
| Só resultados ranqueados, sem LLM | Search API |
| RAG / similaridade | Embeddings API |

## Limitações
- Números lidos em **2026-09-05** em https://docs.perplexity.ai/getting-started/pricing — reconsultar antes de orçar produção.  
- Cotas free do **console** e do **chat**: só da UI no momento do uso — **não inventadas** aqui.  
- Não é aconselhamento fiscal; mapa de pricing público.

## Fontes
1. API Pricing: https://docs.perplexity.ai/getting-started/pricing  
2. Docs overview / índice: https://docs.perplexity.ai · https://docs.perplexity.ai/llms.txt  
3. Console (API ≠ chat): https://console.perplexity.ai  
4. Consumer Max (Help Center): https://www.perplexity.ai/help-center/en/articles/11680686-perplexity-max  
5. Hub pricing consumer: https://www.perplexity.ai/hub/pricing  

## Modelo / cota
- Superfície: **docs oficiais** (WebFetch).  
- Console API: **exhausted** Credits $0 — sem top-up.  
- Chat web Free: sessão assinada; crash anterior `Aw, Snap!` code 9 → neste entregável **não** usado.
