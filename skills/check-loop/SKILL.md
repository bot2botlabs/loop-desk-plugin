---
name: check-loop
description: Fetch one open loop by its ldl_ ID to see its status, due date, and receipt. Use when the user asks about a specific tracked refund, return, or delivery ("what's the status of the Target refund I'm tracking").
---

# Check one loop

Tool: `get_loop` on the `loop-desk` MCP server. It's a read.

## Data shape

`{ loop: { id (ldl_…), kind, counterparty, amount_cents, currency, due_at, notes, external_ref, deadline_desk_receipt_id, source_url, status (open | …), created_at, closed_at, receipt_id } }`

## Steps

1. Get the ID from an earlier `register_loop` result or a `list_due` row. If you only have a merchant name, run `list_due` first and match on `counterparty`.
2. Call `get_loop` with `{ "id": "ldl_…" }`.
3. Report `status`, `due_at` (and whether it has passed), and the public receipt `https://loop.bot2botlabs.com/r/<receipt_id>`.

The MCP server has no close or update tool, so tell the user plainly that resolving a loop isn't available here.

Tool calls need an `ldk_` API key from https://loop.bot2botlabs.com/onboard (`Authorization: Bearer ldk_…`). Loops belong to the key that registered them.
