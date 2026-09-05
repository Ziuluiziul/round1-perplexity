# Brief — Router API vs Agent API: quando usar cada um · set/2026

**Pacote:** R$ 50 · docs-only  
**Data:** 2026-09-05 (America/Sao_Paulo)  
**Público:** makers com SDK OpenAI/Anthropic que precisam escolher endpoint Perplexity sem misturar billing nem citações.  
**Produção:** docs oficiais `docs.perplexity.ai` (WebFetch). API free exhausted — **sem top-up / sem API**.

## Pergunta
Qual a diferença oficial entre **Router API** e **Agent API**, quando usar cada um, e quais URLs/schemas/preços não misturar?

## Resumo acionável
1. **Router** = acesso direto a modelos open-weight hospedados (e catálogo) via schemas OpenAI Chat Completions, OpenAI Responses ou Anthropic Messages. **Sem** web-grounding embutido. Base: `https://api.perplexity.ai/router/v1` (Anthropic SDK: `https://api.perplexity.ai/router` sem `/v1`).
2. **Agent** = respostas com web search, tools, presets, citations. Endpoint: `POST https://api.perplexity.ai/v1/agent`. Use Agent quando precisa de pesquisa citada; Router quando quer só o modelo com seus prompts/tools.
3. Router está em **private preview** — pedir acesso em `api@perplexity.ai`. Mesma `PERPLEXITY_API_KEY`.
4. Router cobra **só por token** (sem fee por request). Agent cobra tokens do modelo (preço do provedor, sem markup) **+** tools (`web_search` $0.0025/inv, `fetch_url` $0.0005, etc.).
5. Failover: Router falha entre **deployments** do mesmo model id automaticamente; Agent usa array `models` (até 5) para fallback **entre modelos/provedores**.

## Tabela — escolha rápida

| Critério | Router API | Agent API |
| --- | --- | --- |
| Objetivo | LLM drop-in (OpenAI/Anthropic SDK) | Pesquisa web + tools + citations |
| Base URL | `/router/v1` (OpenAI) · `/router` (Anthropic SDK) | `/v1/agent` |
| Model id | `creator/model` ex. `perplexity/kimi-k3` | presets ou `openai/…`, `anthropic/…`, etc. |
| Citações / search | Não (só modelo) | Sim (`web_search`, presets) |
| Fee por request | Não | Tools + (Sonar tem request fee à parte) |
| Preview | Private preview | GA nos docs |
| Failover | Deployments do mesmo id | `models[]` até 5 |

## Router — checklist mínimo
| Passo | Ação |
| --- | --- |
| 1 | Pedir acesso preview (`api@perplexity.ai`) |
| 2 | `export PERPLEXITY_API_KEY=…` |
| 3 | OpenAI: `base_url=https://api.perplexity.ai/router/v1` |
| 4 | Anthropic: `base_url=https://api.perplexity.ai/router` (SDK acrescenta `/v1/messages`) |
| 5 | Model: slug do catálogo (`GET /router/v1/models`) |
| 6 | Stream: `stream: true` + `stream_options.include_usage: true` para usage no fim |
| 7 | Billing: sempre taxa do **model id pedido**, independente do deployment |

## Catálogo Router (amostra oficial, USD / 1M tokens)

| Model | Input | Output | Cache read |
| --- | --- | --- | --- |
| `perplexity/nemotron-3.5-lightning-30b-a3b` | 0.0115 | 0.17 | 0.00115 |
| `perplexity/deepseek-v4-flash-0731` | 0.13 | 0.26 | 0.028 |
| `perplexity/glm-5.3-flash` | 0.15 | 0.50 | 0.03 |
| `perplexity/nemotron-3-ultra-550b-a55b` | 0.25 | 2.50 | 0.25 |
| `perplexity/glm-5.2` / `glm-5.3` | 1.40 | 4.40 | 0.14 / 0.26 |
| `perplexity/kimi-k3` | 3.00 | 15.00 | 0.30 |

Fonte: [Router Models & Pricing](https://docs.perplexity.ai/docs/router/models). Catálogo vivo: `GET /router/v1/models`. Model fora do catálogo → `400`.

## Confiabilidade Router (oficial)
- Tráfego ponderado por saúde (erro, capacidade, latência); failover automático entre deployments.
- Erros determinísticos do cliente (`400`, contexto estourado) **não** são retentados.
- Se todos os deployments falham → `429` + `Retry-After`; falha sem output **não** é cobrada.
- Stream: falha pré-primeiro-token é invisível; pós-output → error in-band; cobrar só tokens entregues.
- Multi-turn tenta ficar no mesmo deployment para cache-read.

## Agent — fallback de modelo (contraste)
No Agent, passe `models: ["openai/…", "anthropic/…", …]` (até 5). Array tem precedência sobre `model`. Billing = modelo que **serviu** (`response.model`). Se houver `anthropic/*` na cadeia, inclua `max_output_tokens`. Fonte: [Model Fallback](https://docs.perplexity.ai/docs/agent-api/model-fallback).

## Armadilhas
- Não apontar SDK Anthropic para `/router/v1` (duplica `/v1`).
- Não esperar citations no Router — use Agent / Search.
- Não inventar preços: Router = token-only; tools Agent têm tabela própria em [Pricing](https://docs.perplexity.ai/docs/getting-started/pricing).
- Preview: sem acesso, Router responde negado — não é “exhausted free” no mesmo sentido do console Agent/Sonar.

## Fontes (docs oficiais)
1. https://docs.perplexity.ai/docs/router/quickstart  
2. https://docs.perplexity.ai/docs/router/models  
3. https://docs.perplexity.ai/docs/router/routing-and-reliability  
4. https://docs.perplexity.ai/docs/agent-api/model-fallback  
5. https://docs.perplexity.ai/docs/getting-started/pricing  
6. Índice: https://docs.perplexity.ai/llms.txt  

## PIX LUIZ (R$ 50)
Texto EMV (fonte da verdade):

```
00020126330014br.gov.bcb.pix011108030362994520400005303986540550.005802BR5920LUIZ GUSTAVO CORREIA6009SAO PAULO62080504Pack63044E58
```

Landing: https://ziuluiziul.github.io/round1-perplexity/ · Gist: https://gist.github.com/Ziuluiziul/0deb075aedb06ade0892e6f848e80f69
