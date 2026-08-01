---
name: Attach metadata notes to a Rasgo table
description: Send natural-language notes to a data-warehouse table (FQTN) via the Rasgo API so the Rasgo AI Copilot can learn about the data.
api: openapi/rasgo-openapi.yml
operations: [sendTableMetadata]
method: generated
source: https://docs.rasgoml.com/rasgo-docs/api/table-metadata.md
---

# Attach metadata notes to a Rasgo table

Use the Rasgo API to teach the Rasgo AI Copilot about a table by attaching natural-language
notes to it. Requires an Enterprise Rasgo account.

## Prerequisites
- A personal Rasgo API key (Enterprise plans and above). Copy it from the Rasgo WebApp:
  **User Profile > API Key**. Keep it secret — it grants access to your data.
- The fully-qualified table name (FQTN) in `DATABASE.SCHEMA.TABLE` form.

## Auth
Send the key as a Bearer token, plus the `Rasgo-Client` header:
```
Authorization: Bearer {api_key}
Rasgo-Client: API
Content-Type: application/json
```

## Steps
1. Build the request body for `sendTableMetadata` (`POST /public/table-metadata`):
   ```json
   { "fqtn": "DATABASE.SCHEMA.TABLE", "notes": ["This is a note"] }
   ```
   `notes` accepts one or many strings.
2. `POST` it to `https://api.rasgoml.com/public/table-metadata` with the headers above.
3. On `200`, the response is a confirmation string reporting how many notes were sent and a
   WebApp link to view the changes.

## Rules & gotchas
- The endpoint **de-duplicates**: notes that already exist are detected and skipped, so
  re-sending the same note is safe. There is no separate idempotency-key header.
- The endpoint **only adds** notes. Editing or deleting notes must be done in the WebApp.
- The API is **beta** — endpoint names and contracts may change; re-check the docs if a
  call stops working.
