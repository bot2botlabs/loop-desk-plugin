# Loop Desk plugin (Agent Plugin)

**Tell your agent once about a refund, return, price-adjustment window, or delivery, then ask "what's due?" any time. Every loop gets a public receipt.**

```bash
curl -s https://loop.bot2botlabs.com/mcp -H 'content-type: application/json' \
  -H 'Authorization: Bearer ldk_…' \
  -d '{"jsonrpc":"2.0","id":1,"method":"tools/call","params":{"name":"list_due","arguments":{"before":"2026-12-31T00:00:00Z"}}}'
```

Open-loop tracker by Bot2Bot Labs. Register refunds, returns, price-adjustment windows, deliveries, and unanswered asks with a due date, then list what's due. Every register mints a public receipt.

- MCP: hosted Streamable HTTP at `https://loop.bot2botlabs.com/mcp` (see `mcp.json`)
- Tools: `register_loop`, `get_loop`, `list_due`
- Skills: `track-open-loop`, `whats-due`, `check-loop`

## API key and cost

`initialize` and `tools/list` work without a key. **Tool calls need an `ldk_` key** from https://loop.bot2botlabs.com/onboard, sent as `Authorization: Bearer ldk_…`. Registers are metered (see https://loop.bot2botlabs.com/llms.txt); reads are free. This package stores no key.

## Privacy

Register receipts are public and permanent: kind, counterparty, amount, and due date. Use merchant names only, never personal data.

## Prove it

```bash
LOOP_DESK_API_KEY=<your key> node scripts/prove.mjs
```

Node 22+, no dependencies. The script registers one synthetic loop with fake data and then reads it back. The key is never printed.

Demo receipts: https://loop.bot2botlabs.com/docs/demo

License: MIT
