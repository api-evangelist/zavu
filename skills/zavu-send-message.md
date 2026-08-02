---
name: Send a multi-channel message with Zavu
description: Send an SMS, WhatsApp, Telegram, or email message through Zavu's unified messaging API, safely and idempotently.
api: openapi/zavu-openapi-original.json
operations: [sendMessage, getMessage, listMessages]
---

# Send a message with Zavu

Use this skill to send a message across any channel (SMS, WhatsApp, Telegram, Email) via the Zavu Unified Messaging Layer.

## Auth
- Base URL: `https://api.zavu.dev/v1`
- Header: `Authorization: Bearer <key>` — use a `zv_test_` key while developing (simulates sends, no delivery), a `zv_live_` key in production.

## Steps
1. **Send** — `POST /v1/messages` (`sendMessage`). Provide the `senderId`, the recipient (`to`), the `channel`, and the `content`.
   - Always set an `Idempotency-Key` header. Retrying with the same key returns `409 Conflict` with the original message instead of double-sending.
2. **Confirm** — the response includes the message `id` and `status`. Do not poll aggressively; prefer webhooks (`message.sent`, `message.delivered`, `message.failed`).
3. **Look up later** — `GET /v1/messages/{messageId}` (`getMessage`) for current status; `GET /v1/messages` (`listMessages`) with `cursor`/`limit` to page.

## Error handling
- `400 invalid_request` — fix the payload (e.g. invalid phone number).
- `402` — insufficient balance.
- `429 rate_limited` — honor `Retry-After`; check `X-RateLimit-Remaining` proactively (default 600 req/min).
- On `message.failed`, read `data.errorCode` against `errors/zavu-error-codes.yml` (e.g. `131047` re-engagement required → send an approved template).
