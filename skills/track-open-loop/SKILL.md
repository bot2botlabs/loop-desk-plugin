---
name: track-open-loop
description: Register something the user is waiting on (a refund, a return in progress, a price-adjustment window, a delivery, or an unanswered vendor ask) with a due date, so it can be checked later. Use when the user says "remind me if the refund doesn't show up", "track this return", or "I'm waiting on <company> to reply". Pairs with Deadline Desk receipts.
---

# Track an open loop

Tool: `register_loop` on the `loop-desk` MCP server.

## Data shape

Input: `{ kind (refund | return | price_adjustment | delivery | unanswered | other), counterparty, due_at (ISO-8601 with offset), amount_cents?, currency?, notes?, external_ref?, deadline_desk_receipt_id?, source_url? }`.
Output: `{ loop: { id (ldl_…), status: "open", …input, created_at, receipt_id }, receipt: { id (ldr_…), quoted_claim, public_url }, meter: { recorded, reason } }`.

## Before you call (important)

- **Confirm with the user first.** Registering is billed on the user's own key (about $0.10 each on the meter). Keyless free-tier calls and the shared demo key aren't billed, but keyless loops are public to other keyless users. Say so once.
- **The receipt is public.** `kind`, `counterparty`, the amount, and `due_at` appear on `https://loop.bot2botlabs.com/r/<id>`, and receipts are never deleted. Use a merchant name as `counterparty`, never a person. Keep personal details, order numbers, and addresses out of every field. `notes` isn't on the public receipt, but treat it as stored data anyway.

## Steps

1. Work out `kind`, `counterparty` (the business), and `due_at`, the date by which it should resolve. If the date comes from a Deadline Desk lookup, also pass that `deadline_desk_receipt_id`.
2. Call `register_loop`.
3. Give the user `loop.id`, the due date, and `receipt.public_url`. Keep `loop.id` for later checks.

## Example

`{ kind: "refund", counterparty: "Example Store", amount_cents: 1234, currency: "USD", due_at: "2026-12-15T17:00:00-08:00" }` → `ldl_…`, status `open`, receipt `ldr_…`.

Without a key, tool calls use the free tier (20 per day per IP, shared public demo space, never billed). For private loops, use an `ldk_` API key from https://loop.bot2botlabs.com/onboard (`Authorization: Bearer ldk_…`). Loops belong to the key that registered them.
