---
name: Run a Zavu broadcast campaign
description: Create a broadcast, add contacts in batch, pass AI content review, and send to many recipients with automatic pacing.
api: openapi/zavu-openapi-original.json
operations: [createBroadcast, addBroadcastContacts, sendBroadcast, retryBroadcastReview, getBroadcast]
---

# Run a broadcast with Zavu

Use Broadcasts (not individual `sendMessage` calls) when sending to 100+ recipients — Zavu paces delivery and reserves cost from your balance.

## Steps
1. **Create** — `POST /v1/broadcasts` (`createBroadcast`) with the `senderId` and content.
2. **Add contacts** — `POST /v1/broadcasts/{broadcastId}/contacts` (`addBroadcastContacts`), max 1000 contacts per request; call repeatedly to batch more.
3. **Send** — `POST /v1/broadcasts/{broadcastId}/send` (`sendBroadcast`), immediately or scheduled. Broadcasts pass automated AI content review first.
4. **If rejected** — edit content (`PATCH`), then `POST /v1/broadcasts/{broadcastId}/retry-review` (`retryBroadcastReview`). Max 3 review attempts.
5. **Track** — `GET /v1/broadcasts/{broadcastId}` (`getBroadcast`) for progress/status.

## Notes
- `402` on send means insufficient balance to reserve estimated cost.
- Cancelling (`cancelBroadcast`) skips pending contacts but already-queued messages may still deliver.
