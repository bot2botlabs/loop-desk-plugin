# Loop Desk plugin (Agent Plugin)

**Tell your agent once about a refund, return, price-adjustment window, or delivery, then ask "what's due?" any time. Every loop gets a public receipt.**

```bash
curl -s https://loop.bot2botlabs.com/mcp -H 'content-type: application/json' \
  -d '{"jsonrpc":"2.0","id":1,"method":"tools/call","params":{"name":"list_due","arguments":{"before":"2026-12-31T00:00:00Z"}}}'
```

No key needed for that call: the free tier allows 20 keyless tool calls per day per IP.

Open-loop tracker by Bot2Bot Labs. Register refunds, returns, price-adjustment windows, deliveries, and unanswered asks with a due date, then list what's due. Every register mints a public receipt.

- MCP: hosted Streamable HTTP at `https://loop.bot2botlabs.com/mcp` (see `mcp.json`)
- Tools: `register_loop`, `get_loop`, `list_due`
- Skills: `track-open-loop`, `whats-due`, `check-loop`

## API key and cost

**Free tier, no key:** up to 20 tool calls per day per IP (resets 00:00 UTC). Keyless calls run in the shared public demo space and are never billed, so anything you register there is visible to other keyless users. Each keyless response includes a `free_tier` object with `used`, `remaining` and `resets_at`. Past the cap, the server returns JSON-RPC error `-32029` with a link to the free-key page.

**Your own loops:** get a free `ldk_` key (no card) from https://loop.bot2botlabs.com/onboard and send `Authorization: Bearer ldk_…`. Keyed registers are metered (see https://loop.bot2botlabs.com/llms.txt); reads are free. This package stores no key.

Terms: https://loop.bot2botlabs.com/terms · Privacy: https://loop.bot2botlabs.com/privacy

## Privacy

Register receipts are public and permanent: kind, counterparty, amount, and due date. Use merchant names only, never personal data.

## Prove it

```bash
LOOP_DESK_API_KEY=<your key> node scripts/prove.mjs
```

Node 22+, no dependencies. The script registers one synthetic loop with fake data and then reads it back. The key is never printed.

Demo receipts: https://loop.bot2botlabs.com/docs/demo

License: MIT

---

## About Bot2Bot Labs

Bot2Bot Labs publishes [dated AI receipts](https://bot2botlabs.com/receipts) — what AI-built software costs to build and run, how long it took, what broke, and what subscription it replaces.

- Site: [bot2botlabs.com](https://bot2botlabs.com)
- For agents: [bot2botlabs.com/agents](https://bot2botlabs.com/agents)
- X: [@bot2botlabs](https://x.com/bot2botlabs)
- Contact: jordan@bot2botlabs.com

Measured. Dated. Reproducible. No paid placement.
