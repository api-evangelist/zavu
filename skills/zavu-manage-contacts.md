---
name: Manage Zavu contacts and channels
description: Create contacts, add communication channels, look up by phone, and merge duplicates in Zavu.
api: openapi/zavu-openapi-original.json
operations: [createContact, getContactByPhone, addContactChannel, mergeContacts, listContacts]
---

# Manage contacts with Zavu

## Steps
1. **Create** — `POST /v1/contacts` (`createContact`) with the contact's initial channel(s).
2. **Add a channel** — `POST /v1/contacts/{contactId}/channels` (`addContactChannel`) to attach another address (SMS/WhatsApp/email/Telegram) to the same person.
3. **Look up by phone** — `GET /v1/contacts/phone/{phoneNumber}` (`getContactByPhone`) before creating, to avoid duplicates.
4. **Merge duplicates** — `POST /v1/contacts/{contactId}/merge` (`mergeContacts`) with the `sourceContactId` to fold one contact into another.
5. **List/page** — `GET /v1/contacts` (`listContacts`) with `cursor`/`limit`; filter with `contains`.

## Notes
- Auth: `Authorization: Bearer <key>` against `https://api.zavu.dev/v1`.
- Use the phone introspection endpoint (`introspectPhone`, `POST /v1/introspect/phone`) to validate/enrich a number (line type, carrier) before adding it.
