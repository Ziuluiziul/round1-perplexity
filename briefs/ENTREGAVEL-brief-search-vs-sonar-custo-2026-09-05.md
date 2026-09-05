# Brief — Search API vs Sonar: quando pagar US$5/1k vs tokens+request · set/2026

**Pacote:** Brief curto (R$ 20) — docs-only  
**Fonte:** https://docs.perplexity.ai/docs/getting-started/pricing.md  
**Produção:** free/API parado → WebFetch docs. Sem inventar RPM.

## Pergunta
Quando usar **Search API** (resultados crus) em vez de **Sonar** (resposta gerada + busca)?

## Resumo acionável
1. **Search API** = US$ **5,00 / 1.000** `POST /search` OK; até **5 queries** no mesmo POST = **1** unidade; sem fee de token.  
2. **Sonar** = tokens (ex. Sonar US$1/1M in+out) **+** request fee Low/Med/High (**5 · 8 · 12** /1k).  
3. Use Search se você **orquestra** o LLM em outro lugar e só precisa de hits ranqueados.  
4. Use Sonar/Agent se precisa de **resposta grounded** numa chamada.  
5. Invalid/rate-limit/upstream fail no Search **não** cobram; resposta vazia bem-sucedida **cobra**.

## Mini-tabela
| Caminho | Unidade | Preço publicado |
|---------|---------|-----------------|
| Search API | 1 POST /search (≤5 queries) | US$ 5 / 1k |
| Sonar Low | 1 request + tokens | US$ 5 / 1k + tokens |
| Sonar Pro Fast Low | 1 request + tokens | US$ 6 / 1k + $3/$15 per 1M |

## Limites
Docs não fixam “melhor modelo” genérico — depende do pipeline. Confira Pricing oficial antes de orçar.

## PIX
R$20 EMV `pix-r20` (tag54=20) nas Pages. Após PIX: comprovante.
