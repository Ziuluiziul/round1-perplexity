# Brief — Wide Research (preset `wide-research`) · set/2026

**Pacote:** R$ 50 · docs-only  
**Data:** 2026-09-05 (America/Sao_Paulo) ~23:09  
**Público:** makers que precisam de listas grandes com evidência por item (empresas, pessoas, filings) sem orquestrar o loop na mão.  
**Produção:** docs oficiais `docs.perplexity.ai` (WebFetch). API free exhausted — **sem top-up / sem API**.

## Pergunta
Quando usar o preset **`wide-research`**, como submeter em background, e o que pedir no `input` para sair com arquivo citável?

## Resumo acionável
1. **`wide-research`** = preset para coleções *wide* (muitos itens) **e** *deep* (cada claim com fonte). É a forma medida pelo benchmark **WANDR** (Wide ANd Deep Research) da Perplexity.
2. Runs demoram **minutos** → sempre `background=true` + poll por `id` até status terminal: `completed` | `failed` | `cancelled` | `incomplete`.
3. O agente **escreve arquivo** no sandbox e entrega via `share_file` → listar/baixar com endpoints de files (`responses.files.list` / `.content`).
4. Qualidade = precisão do prompt: alvo numérico (“≥70”), regras de qualificação (datas, geo), **1 URL autoritativa por registro**, formato de saída (ex. `results.jsonl` + campos).
5. Não confundir com `high`/`xhigh` “só pesquisa longa”: wide-research é **coleção estruturada** + download de ficheiro.

## Checklist de prompt (copiar)
| # | Incluir no `input` |
| --- | --- |
| 1 | Quantidade mínima (`at least N`) |
| 2 | Critérios de inclusão/exclusão |
| 3 | Fonte obrigatória por linha (URL autoritativa) |
| 4 | Nome do ficheiro de saída |
| 5 | Schema dos campos (JSONL / CSV / MD) |

## Fluxo oficial (3 passos)
1. `responses.create(preset="wide-research", background=True, input=…)` → guarda `id`  
2. Poll `responses.retrieve(id)` a cada ~5s até terminal  
3. `responses.files.list(id)` → `files.content(file_id=…)` → `write_to_file`

## Curl mínimo
```bash
curl https://api.perplexity.ai/v1/agent \
  -H "Authorization: Bearer $PERPLEXITY_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"preset":"wide-research","background":true,"input":"Find at least 20 … Write results.jsonl with fields: …"}'
```

## Quando NÃO usar
| Situação | Preferir |
| --- | --- |
| 1 fato / resumo rápido | preset `fast` |
| Pesquisa multi-step leve | `low` / `medium` |
| PDF/XLSX com skill office | `skills` + `office/pdf` (docs Skills) |
| Só hits ranqueados, sem LLM | Search API |

## Armadilhas
- Submeter **sem** `background=true` → cliente pode cair no meio.  
- Prompt vago (“liste startups”) → coleção rasa / sem fonte.  
- Ignorar ficheiros e ler só `output_text` → perde o entregável.  
- Nesta missão receita: **API $0** → este brief é mapa docs-only; execução paga exige créditos (sem top-up até Luiz).

## Fontes (docs oficiais)
1. https://docs.perplexity.ai/docs/agent-api/wide-research  
2. https://docs.perplexity.ai/docs/agent-api/background-mode  
3. https://docs.perplexity.ai/docs/agent-api/working-with-files  
4. https://docs.perplexity.ai/docs/agent-api/presets  
5. Índice: https://docs.perplexity.ai/llms.txt  

## PIX LUIZ (R$ 50)
Texto EMV (fonte da verdade — tag54=50.00):

```
00020126330014br.gov.bcb.pix011108030362994520400005303986540550.005802BR5920LUIZ GUSTAVO CORREIA6009SAO PAULO62080504Pack63044E58
```

Landing: https://ziuluiziul.github.io/round1-perplexity/ · Space: https://huggingface.co/spaces/Ziulluizziul/round1-perplexity-pix · CTA: https://telegra.ph/Perplexity-briefs--R20--R50-PIX-LUIZ-09-05  

Após PIX, envie o comprovante.
