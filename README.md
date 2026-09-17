# SnowSignals MCP server

[![Listed on mcpservers.org](https://mcpservers.org/badge.svg)](https://mcpservers.org/servers/snowkidind/snowsignals-mcp)

SnowSignals is a market-data service you reach over the [Model Context Protocol](https://modelcontextprotocol.io).
Point an MCP client at one hosted endpoint and your agent can read where the market
currently sits, then look up how that state has tended to play out.

This repo is the connection guide and the published server manifest. There's no server
code to install here. The server is hosted, and you pay per call from a prepaid balance
topped up in stablecoin. Two of the tools are free and need no account, so you can try it
before you fund anything.

- Website: https://snowsignals.io
- Endpoint: `https://snowsignals.io/mcp` (Streamable HTTP)
- Machine-readable summary for agents: https://snowsignals.io/llms.txt
- Human docs: https://snowsignals.io/docs

## What it gives you

The current product is **TrendVane**, a multi-timeframe market-phase model. A *phase* is a
label for the state a market is in right now for a given currency and timeframe. TrendVane
reports the state, not a forecast and not a trade signal. You bring your own strategy; it
tells you which regime you're in.

Two reads sit behind that:

- The **live phase** for a currency across timeframes, either the last closed-bar reading
  (deterministic) or the intra-bar reading (fresher, still forming).
- A **resolution model** built from BTC history that shows how each phase has historically
  moved on: which phase tends to follow which, how often a trend holds, and the reward
  versus drawdown that came with it. Every number carries its sample count.

More data products get added over time, so the name is deliberately general.

## Connecting a client

Most MCP clients take a remote server as a URL. In a client that reads a JSON config
(Claude Desktop and similar), add:

```json
{
  "mcpServers": {
    "snowsignals": {
      "url": "https://snowsignals.io/mcp"
    }
  }
}
```

Call `trendvane_list_phase_meta` first. It's free, needs no login, and returns the enabled phases,
timeframes and currencies along with the pricing model, so your agent can work out what a
paid call costs before making one.

When you call a metered tool, the server answers an unauthenticated request with an OAuth
challenge and the client walks you through sign-in. Scope is `daas:read`. You can also mint
a static token in the web dashboard for headless setups.

## The tools

Free, no account needed:

- `trendvane_list_phase_meta` — the phases, timeframes, currencies, and pricing. Start here.
- `trendvane_phase_resolution_stats` — the BTC-derived model of how each phase resolves. This is the
  interpretive layer for a live reading.

Your own account, free but signed in:

- `system_get_balance` — prepaid balance, what's on hold, and your deposit addresses.
- `system_get_usage` — your metered-call history.
- `system_get_notifications` — account, billing and support messages.
- `system_deposit_poll` — nudge the wallet to look for a deposit you just sent.

Metered, priced per call:

- `trendvane_get_phase` — the phase for one currency across one or all timeframes.
- `trendvane_compose_phases` — many currencies and timeframes in a single call, priced by how many
  readings come back.

## Pricing

Prepaid and metered. You fund a balance in stablecoin, and each paid call draws down from
it based on how many readings it returns. The exact base rate and multipliers come back
from `trendvane_list_phase_meta`, so the price is visible before you spend. The two discovery tools
never cost anything.

## Manifest

[`server.json`](./server.json) is the entry published to the
[official MCP registry](https://registry.modelcontextprotocol.io) under `io.snowsignals`.

## Support

Questions or access requests: [@snowkidind](https://github.com/snowkidind).

## License

This repo — the connection docs and the `server.json` manifest — is under the
[Apache License 2.0](LICENSE). The **SnowSignals** and **TrendVane** names and the brand
assets in `media/` are trademarks of snowkidind and are not covered by that license (see
[NOTICE](NOTICE)); linking to the service is always fine.
