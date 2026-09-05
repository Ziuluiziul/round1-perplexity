# Brief — Agent API Output Control: Streaming SSE + Structured Outputs · set/2026

**Pacote:** R$ 50 · docs-only  
**Data:** 2026-09-05 (America/Sao_Paulo)  
**Público:** makers que precisam de UI em tempo real ou JSON tipado sem scrapar prosa.  
**Produção:** docs oficiais `docs.perplexity.ai` (WebFetch). API free exhausted — **sem top-up / sem API**.

## Pergunta
Como ativar **streaming (SSE)** e **structured outputs (JSON Schema)** no Agent API, e quais armadilhas oficiais evitar?

## Resumo acionável
1. **Streaming:** `stream=true` em `POST https://api.perplexity.ai/v1/agent` → `text/event-stream`. Texto parcial vem em eventos `response.output_text.delta`; fim em `response.completed` (com usage).
2. Streaming vale para **todos** os models/presets do Agent API.
3. **Structured outputs:** campo `response_format` com `type: json_schema` + `json_schema.name` (1–64 alfanumérico) + `schema` JSON Schema válido.
4. Propriedades em `required` são obrigatórias; omitidas de `required` podem ser `null`. Sem `required` → todas opcionais. Igual em stream e não-stream (`/v1/agent` e alias `/v1/responses`).
5. **1ª request** com schema novo: delay típico **10–30s** no first token (preparação do schema); pode timeout. Requests seguintes sem esse delay.
6. **Links dentro do JSON:** não confiar — risco de URL inventada/quebrada. Usar `citations` / `search_results` da resposta.
7. Runs longos (minutos): preferir `background=true` em vez de manter SSE aberto (ver brief Background Mode).

## Streaming — checklist
| Passo | Ação |
|-------|------|
| 1 | `stream: true` no body |
| 2 | Iterar eventos SSE |
| 3 | Imprimir/UI em `response.output_text.delta` |
| 4 | Ler usage em `response.completed` |
| 5 | Tratar `RateLimitError` / connection errors (SDK) |

Endpoint curl oficial: `https://api.perplexity.ai/v1/agent` com header `Authorization: Bearer $PERPLEXITY_API_KEY`.

## Structured outputs — checklist
| Campo | Regra |
|-------|-------|
| `response_format.type` | `json_schema` |
| `json_schema.name` | obrigatório, 1–64 chars alfanuméricos |
| `schema` | JSON Schema válido; `additionalProperties: false` nos exemplos |
| Prompt | dar dica do formato (“return JSON with …”) melhora compliance |
| Links | pegar de `search_results`/`citations`, não do JSON gerado |

## Quando usar o quê
| Objetivo | Caminho |
|----------|---------|
| Chat UI com tokens chegando | `stream=true` |
| Pipeline / extrair campos tipados | `response_format` JSON Schema |
| Pesquisa longa / sandbox | `background=true` (não SSE interminável) |
| Citações progressivas no stream | cookbook Streaming Citation Parsing |

## Limitações
- Docs lidos em **2026-09-05** — reconsultar antes de orçar produção.
- Sem chamada API nesta entrega (créditos $0).
- Não é SDK tutorial completo: mapa docs-only.

## Fontes (URLs oficiais)
1. Output Control: https://docs.perplexity.ai/docs/agent-api/output-control  
2. Structure the output: https://docs.perplexity.ai/docs/agent-api/building-agents/shape-output  
3. Agent API reference (SSE events): https://docs.perplexity.ai/api-reference/agent-post  
4. Streaming Citation Parsing: https://docs.perplexity.ai/docs/cookbook/articles/streaming-citations/README  
5. Structured Output Extraction: https://docs.perplexity.ai/docs/cookbook/articles/structured-output-extraction/README  
6. Índice docs: https://docs.perplexity.ai/llms.txt  

## PIX
R$50 EMV Pages (`qr_2` / `pix-r50`). Hosts: Pages · Space · Telegra. Comprovante após PIX.
