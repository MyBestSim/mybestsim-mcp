# Installing the MyBestSim MCP server

MyBestSim is a hosted, remote MCP server. There is nothing to clone, build or install, and no API key.

- URL: `https://mcp.mybestsim.com/mcp`
- Transport: Streamable HTTP
- Authentication: none

## Cline

Add this entry to `cline_mcp_settings.json`, keeping any servers already there:

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

## Check that it works

After saving, the server should list four tools: `search_esim_plans`, `plan_trip_esim`, `get_destination_overview` and `get_esim_plan`. Try: "I need an eSIM for 10 days in Japan with at least 5 GB of data."
