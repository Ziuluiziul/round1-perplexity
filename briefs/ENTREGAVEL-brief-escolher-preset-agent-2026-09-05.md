# Brief — Qual preset Agent usar (fast→xhigh) · set/2026

**Pacote:** R$ 50 · docs-only  
**Fonte:** https://docs.perplexity.ai/docs/agent-api/presets.md

## Tabela oficial (uso)
| Preset | Bom para |
|--------|----------|
| fast | 1 fato / resumo rápido / latência |
| low | pesquisa leve multi-step + citações |
| medium | multi-hop / muitas fontes |
| high | cobertura profunda / análise institucional |
| xhigh | loops longos + sandbox + finance_search |
| wide-research | coleções grandes evidence-backed |

## Regras de ouro
1. Nome dinâmico (`preset="low"`) pega updates oficiais; freeze = copiar valores e omitir `preset`.  
2. Overrides: `model`, `max_steps`, `reasoning`; `tools` **merge** por tool.  
3. Renomes: fast-search→fast, pro-search→low, deep-research→medium, advanced→high, ultra→xhigh.  
4. Updates miram mesmo band de custo/latência; qualidade sobe.  
5. Não setar `preset` e `profile` juntos.

## Exercício
Mapear 5 queries reais do seu produto → 1 preset cada + 1 frase de justificativa.

## PIX
R$50 EMV Pages/curso.
