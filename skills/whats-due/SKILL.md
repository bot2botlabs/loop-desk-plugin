---
name: whats-due
description: List the user's open loops (refunds, returns, price-adjustment windows, deliveries, unanswered asks) that are due on or before a date. Use for "what am I still waiting on", "anything due this week", or a weekly follow-up sweep.
---

# What's due

Tool: `list_due` on the `loop-desk` MCP server. It's a read, and free on the key.

## Data shape

`{ before, loops: [ { id, kind, counterparty, amount_cents, currency, due_at, status, notes, receipt_id, … } ] }`

## Steps

1. Pick the cutoff: `before` is an ISO datetime with offset. Use the end of the day, week, or month the user means, in their time zone.
2. Call `list_due` with `{ "before": "<ISO>" }`.
3. Sort by `due_at`. Show overdue items first (`due_at` is already past), then upcoming ones. Give each one's kind, counterparty, amount (`amount_cents / 100` plus currency), and due date.
4. For an overdue refund or return, suggest the next step (contact the merchant, check card statements). If it came from a store policy, offer a Deadline Desk lookup for the deadline.

## Note

Results are scoped to the API key. Keyless calls, or a shared or demo key, show everyone's demo loops, so check whose key it is before you read the list back as "yours".

Without a key, tool calls use the free tier (20 per day per IP, shared public demo space, never billed). For private loops, use an `ldk_` API key from https://loop.bot2botlabs.com/onboard (`Authorization: Bearer ldk_…`). Loops belong to the key that registered them.
