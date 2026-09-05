# Brief — Rate limits & Usage Tiers (Tier 0) · set/2026

**Pacote:** R$ 20 · docs-only  
**Fonte:** https://docs.perplexity.ai/docs/admin/rate-limits-usage-tiers.md

## Tiers (créditos cumulativos comprados)
| Tier | Créditos | Nota |
|------|----------|------|
| 0 | $0 | contas novas |
| 1 | $50+ | | 
| 2 | $250+ | |
| 3 | $500+ | |
| 4 | $1.000+ | |
| 5 | $5.000+ | permanente após atingir |

## Agent API (QPS + RPM independentes)
| Tier | QPS | RPM |
|------|-----|-----|
| 0 | 1 | 50 |
| 1 | 3 | 150 |
| 2 | 8 | 500 |
| 3 | 17 | 1.000 |
| 4 | 33 | 4.000 |
| 5 | 33 | 8.000 |

## Search API (todas as tiers)
- `POST /search`: **50** query-units/s (burst 50).
- Multi-query: **1** billing unit / request, mas **1 rate-unit por query** no array.
- 429 Router: `Retry-After`; **não cobrado**.

## Sonar Tier 0 (legado até 2026-09-27)
`sonar`/`sonar-pro`/`sonar-reasoning-pro` **50** RPM; `sonar-deep-research` e `POST /v1/async/sonar` **5** RPM.

## Prática
Leaky bucket (burst = capacidade). Exponential backoff + jitter em 429. Sem top-up nesta missão → operar em **Tier 0**.

## PIX
R$20 EMV Pages. Comprovante após PIX.
