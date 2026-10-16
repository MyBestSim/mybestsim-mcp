<p align="center">
  <img src="assets/logo.png" alt="MyBestSim logo" width="120" height="120">
</p>

# MyBestSim MCP server

Compare prepaid travel eSIM plans by destination, data allowance and trip length, and plan eSIMs for multi-country trips, from any AI assistant that supports the [Model Context Protocol](https://modelcontextprotocol.io/).

[MyBestSim](https://mybestsim.com/?utm_source=github&utm_medium=mcp-directory&utm_campaign=mybestsim-mcp) is an independent comparator of prepaid travel eSIM plans. This hosted MCP server gives an assistant read-only access to the plans listed on the comparison pages of mybestsim.com.

| | |
| --- | --- |
| Endpoint | `https://mcp.mybestsim.com/mcp` |
| Transport | Streamable HTTP |
| Authentication | None |
| Access | Read only, free, no account |
| Official MCP Registry | `com.mybestsim/esim-comparator` |

This repository only documents the hosted server and holds its registry metadata ([`server.json`](server.json)). There is nothing to install or run locally.

## Tools

| Tool | What it does |
| --- | --- |
| `search_esim_plans` | Shortlists eSIM plans for one country or region. Filters: data allowance (or a light, standard or heavy daily use), trip length, unlimited data, maximum price, 5G, hotspot, calls and SMS. Returns up to 10 plans, at most 2 per provider, with data, validity, price in USD (and EUR when known) and the link to the comparison page. |
| `plan_trip_esim` | Plans a trip through several countries or regions (up to 8 stages, in travel order). Compares, at the real total price, one eSIM covering every stage against one eSIM per stage and the combinations in between, then recommends an option. |
| `get_destination_overview` | Summarises the eSIM market for a destination: number of plans and providers listed, lowest price, lowest price per GB, lowest priced unlimited plan, and available validities. |
| `get_esim_plan` | Returns the detail of one plan found by the tools above: coverage (country count, region), features, validity and price. |

All tools are annotated `readOnlyHint: true` and `destructiveHint: false`.

## Connect

### Claude (claude.ai and Claude Desktop)

Settings > Connectors > Add custom connector, then paste:

```
https://mcp.mybestsim.com/mcp
```

No authentication is needed.

If you prefer the Claude Desktop configuration file (`claude_desktop_config.json`), which only starts local processes, bridge the remote server with [`mcp-remote`](https://www.npmjs.com/package/mcp-remote):

```json
{
  "mcpServers": {
    "mybestsim": {
      "command": "npx",
      "args": ["-y", "mcp-remote", "https://mcp.mybestsim.com/mcp"]
    }
  }
}
```

### Claude Code

```bash
claude mcp add --transport http mybestsim https://mcp.mybestsim.com/mcp
```

### ChatGPT (developer mode)

1. Settings > Apps & Connectors > Advanced settings, turn on Developer mode.
2. Create a connector, name it MyBestSim, and set the MCP server URL to `https://mcp.mybestsim.com/mcp`.
3. Choose "No authentication".

Results that come from `search_esim_plans` and `plan_trip_esim` are also shown as a visual card in ChatGPT.

### Cursor

`~/.cursor/mcp.json` (or `.cursor/mcp.json` in a project):

```json
{
  "mcpServers": {
    "mybestsim": {
      "url": "https://mcp.mybestsim.com/mcp"
    }
  }
}
```

### VS Code

`.vscode/mcp.json`:

```json
{
  "servers": {
    "mybestsim": {
      "type": "http",
      "url": "https://mcp.mybestsim.com/mcp"
    }
  }
}
```

### Cline

`cline_mcp_settings.json`:

```json
{
  "mcpServers": {
    "mybestsim": {
      "type": "streamableHttp",
      "url": "https://mcp.mybestsim.com/mcp",
      "disabled": false
    }
  }
}
```

### Windsurf

`~/.codeium/windsurf/mcp_config.json`:

```json
{
  "mcpServers": {
    "mybestsim": {
      "serverUrl": "https://mcp.mybestsim.com/mcp"
    }
  }
}
```

### Any other client

Point a Streamable HTTP client at `https://mcp.mybestsim.com/mcp`. The server answers `initialize`, `ping`, `tools/list`, `tools/call`, `resources/list`, `resources/templates/list` and `resources/read`.

## Example questions

- "I need an eSIM for 10 days in Japan with at least 5 GB of data."
- "Two weeks in France, Italy and Spain: one eSIM for the whole trip, or one per country?"
- "What does a travel eSIM usually cost in Thailand?"

When the trip length or the data need is missing, the server assumes a 7-day trip with standard use and says so in its answer, so the assistant can ask you and refine.

## Limitations

- Prices are indicative. They come from provider catalogs refreshed daily and can change on the provider's site before checkout.
- Nothing can be bought inside the conversation. Each result links to the MyBestSim comparison page for the destination, where you compare and continue to the provider's own site.
- MyBestSim does not sell, activate or support eSIMs. Covered: prepaid travel eSIMs for visitors. Not covered: domestic or resident mobile plans.
- Unlimited plans are shown as "Unlimited". Check the provider's fair use terms before buying.
- The endpoint is rate limited to protect the service.

## Affiliation and ranking

MyBestSim earns a commission when a traveler buys through its links.

Results are the plans listed on the MyBestSim comparison page that each link opens. The server ranks them only by the request (price, value for the data need, data or validity): no paid placement changes their order.

Which plans a page lists follows the comparator's rules: near-identical plans from one provider may be shown once, and when plans from different providers are comparable, the page may keep only the one from a provider that earns MyBestSim more, sometimes at a higher price. A plan that a provider pays to feature is never removed by these rules, and it gets no better rank from the server.

## Documentation and privacy

- Documentation: [MyBestSim in ChatGPT and Claude](https://mybestsim.com/en/mybestsim-in-chatgpt-and-claude/?utm_source=github&utm_medium=mcp-directory&utm_campaign=mybestsim-mcp)
- Privacy: [privacy policy, MyBestSim app for ChatGPT and Claude](https://mybestsim.com/en/privacy-policy/?utm_source=github&utm_medium=mcp-directory&utm_campaign=mybestsim-mcp#mybestsim-app-chatgpt-claude)

The server does not require an account and does not ask for personal data. Technical request data, such as the IP address used for rate limiting, is handled as described in the privacy policy.

## Support

Open an issue in this repository, or use the contact details on [mybestsim.com](https://mybestsim.com/?utm_source=github&utm_medium=mcp-directory&utm_campaign=mybestsim-mcp).

## License

The text of this README, `llms-install.md` and the configuration files in this repository (`server.json` and the JSON snippets above) are released under the [MIT License](LICENSE). The MyBestSim name, logo and the hosted service are not covered by this license.
