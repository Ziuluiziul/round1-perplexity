# Brief — Migrar Sonar → Agent API (checklist field-by-field) · set/2026

**Pacote:** R$ 50 · docs-only  
**Data:** 2026-09-05 (America/Sao_Paulo)  
**Público:** quem ainda chama `chat.completions` / `/v1/sonar` e precisa migrar antes do fim do suporte Sonar (27 set 2026).  
**Produção:** docs oficiais `docs.perplexity.ai` (WebFetch). API free exhausted — **sem top-up / sem API**.

## Pergunta
Como migrar uma integração **Sonar Chat Completions** para o **Agent API** sem perder web search, e o que muda em endpoint, body, output, filtros e async?

## Resumo acionável
1. Sonar Chat Completions **é** Agent API agora; Sonar permanece suportado **até 27 set 2026**. Docs recomendam migrar o existente e usar Agent em projetos novos.
2. Endpoint: `POST https://api.perplexity.ai/v1/sonar` → `POST https://api.perplexity.ai/v1/agent` (alias OpenAI SDK: `/v1/responses`). SDK: `chat.completions.create` → `responses.create`.
3. Body: `messages` → `input` (string simples **ou** array de items). System → `instructions` (ou item `role: system` em `input`). Model Sonar curto (`sonar`) → id Agent `perplexity/sonar` **ou** preferir **preset** (`fast`/`low`/`medium`/`high`/`xhigh`).
4. Output: `choices[0].message.content` → `response.output_text`. Fontes/steps ficam no array tipado `output` (`search_results`, `message`, sandbox, etc.).
5. Web search **não** é mais implícito: sem preset, inclua `tools: [{"type":"web_search", "filters":{…}}]`. Filtros Sonar de topo (`search_domain_filter`, `search_recency_filter`, …) vão para `filters` do tool.
6. Async Sonar → `background: true` + poll por `id` (Background Mode). Stream: consumir SSE tipado `response.output_text.delta` (não `delta.content` do chat).
7. Atalho OpenAI SDK: `base_url=https://api.perplexity.ai/v1` + `client.responses.create`; presets via `extra_body={"preset":"low"}`.

## Mapa de modelos → presets (oficial)

| Sonar Chat Completions | Agent preset | Melhor para |
| --- | --- | --- |
| Sonar | `fast` | Lookup / definição / resumo rápido |
| Sonar Pro | `low` | Pesquisa cotidiana multi-step leve |
| Sonar Reasoning Pro | `medium` | Multi-hop / agregação ampla |
| Sonar Deep Research | `high` | Cobertura profunda / expert |
| (SOTA deep) | `xhigh` | Máxima qualidade nos benches oficiais |

## Checklist de migração (ordem)

| # | Ação |
| --- | --- |
| 1 | Trocar URL para `/v1/agent` (ou `/v1/responses` via OpenAI SDK) |
| 2 | Trocar método SDK para `responses.create` |
| 3 | `messages` → `input`; system → `instructions` |
| 4 | Escolher `preset` **ou** `model` (`perplexity/sonar` se quiser paridade mínima) |
| 5 | Se sem preset: adicionar `tools` com `web_search` + filtros |
| 6 | Ler `output_text`; iterar `output[]` se precisar de citations/tools |
| 7 | Stream: branch em `event.type == response.output_text.delta` |
| 8 | Async: `background: true` + GET pelo `id` |
| 9 | Multi-turn: `previous_response_id` (atalho) ou replay em `input` |
| 10 | Renomear `max_tokens` → `max_output_tokens` |

## Parâmetros — o que muda / some

**Direto (renomeado ou movido)**  
- Filtros de domínio/recência/data → `web_search.filters` (mesmos nomes)  
- `web_search_options.search_context_size` / `user_location` → campos do tool `web_search` (fora de `filters`)  
- `num_search_results` → `max_results` no tool  
- `reasoning_effort` → `reasoning.effort`  
- `stream` permanece  

**Sem 1:1 — redesenhar**  
- `disable_search` → omitir `web_search` (em preset: `tools: []` **não** limpa tools do preset; workaround bruto `max_tool_calls: 0`)  
- `search_mode: academic` → domain filters + prompt; `sec` → Finance Search ou filtro SEC  
- `search_type` Pro Search → mapear `"fast"`→preset `fast`, `"pro"`→preset `low`  
- `return_related_questions` → pedir no prompt / JSON Schema  
- `response_format.type: regex` → **sem** equivalente; usar `json_schema`  

**Drop (sem equivalente Agent)**  
- `search_language_filter`  
- `stream_mode` (`full` vs `concise`)  
- Image/video results (`return_images`, `return_videos`, …)  
- Multimodal `file_url` / `pdf_url` / `video_url` (imagem: `input_image`)  

## Curl mínimo Agent (preset)

```bash
curl https://api.perplexity.ai/v1/agent \
  -H "Authorization: Bearer $PERPLEXITY_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"preset":"fast","input":"What are the latest developments in AI agents?"}'
```

## OpenAI SDK (compat)

```python
from openai import OpenAI
client = OpenAI(api_key=os.environ["PERPLEXITY_API_KEY"],
                base_url="https://api.perplexity.ai/v1")
r = client.responses.create(input="…", extra_body={"preset": "low"})
print(r.output_text)
```

## Armadilhas
- Esquecer `web_search` em config custom = resposta **sem** grounding (só knowledge do modelo).  
- Ler só texto e ignorar `output` tipado perde auditoria de searches/sandbox.  
- `temperature`/`top_p` em famílias GPT-5 / o1 / o3 no Agent: **ignorados** silenciosamente.  
- `temperature: 0` **não** garante output byte-idêntico.  
- Deadline Sonar: **2026-09-27**.  
- Skill oficial de migração (coding agents): repo `perplexityai/api-platform-developers` → `skills/migrate-sonar-to-agent-api`.

## Fontes (docs oficiais)
1. https://docs.perplexity.ai/docs/agent-api/migrate-from-sonar/overview  
2. https://docs.perplexity.ai/docs/agent-api/migrate-from-sonar/how-to  
3. https://docs.perplexity.ai/docs/agent-api/migrate-from-sonar/benchmarks  
4. https://docs.perplexity.ai/docs/agent-api/openai-compatibility  
5. https://docs.perplexity.ai/docs/agent-api/background-mode  
6. Índice: https://docs.perplexity.ai/llms.txt  

## PIX LUIZ (R$ 50)
Texto EMV (fonte da verdade):

00020126330014br.gov.bcb.pix011108030362994520400005303986540550.005802BR5920LUIZ GUSTAVO CORREIA6009SAO PAULO62080504Pack63044E58

Recebedor: LUIZ · valor R$ 50 (`pix_copia_cola_2` / `qr_2`). Enviar comprovante após PIX.
