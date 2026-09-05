# Brief — Agent API Background Mode (async) · set/2026

**Pacote:** R$ 20 · docs-only  
**Fonte:** https://docs.perplexity.ai/docs/agent-api/background-mode.md

## Resumo
1. Runs longos (deep research / sandbox) → `background=true` em `POST https://api.perplexity.ai/v1/agent`.
2. Create devolve Response com `id` + `status` na hora; o job continua no servidor mesmo se o cliente cair.
3. Poll: `GET /v1/agent/{id}` até status terminal: `completed` | `failed` | `cancelled` | `incomplete`.
4. Não-terminal: `queued` | `in_progress`. Intervalo típico de poll nos exemplos oficiais: **2s**.
5. Stream + reconnect: `background=true` + `stream=true`; guardar `sequence_number`; retomar com `?stream=true&starting_after=N`. Se janela expirou → **400** → cair para GET snapshot.
6. Cancel: `POST /v1/agent/{id}/cancel` → `200` com `cancelling`; poll até `cancelled`. Já terminal → 400; id alheio/inexistente → 404.

## PIX
R$20 EMV Pages (`pix-r20`). Comprovante após PIX.
