# Plain MCP-Server Demo

Want to have ChatGPT manage your customer support? You've come to the right
place!

## Setup

1. First, you'll need to get the apollo-mcp-server binary. Use the command below to
install the binary for both Mac OS or Linux.

```bash
curl -sSL https://mcp.apollo.dev/download/nix/latest | sh
```

2. Next, install the node dependencies for the demo app:

```bash
pnpm install
```

3. Update the `apollo-config.yaml`

You'll need to add at least your Plain API token and probably update the GraphQL
API URL.

4. Finally, running the setup depends on what you're wanting to do.

- If you want to just start something and debug the MCP server, ensure the `transport` in `apollo-config.yaml` is set to
  `streamable_http`. You can then run `pnpm:dev` which will start 2 things, the
  mcp server and the mcp-inspector project (analogous to GraphiQL). Use `http://localhost:5001/mcp`
  as the MCP server URL in the inspector web ui.
- To expose this MCP server to OpenAI API compatible LLM's, you have to leverage
  the mcp-openai-proxy. This exposes our apollo-mcp-server as an OpenAI model
  -compatible API.
    - For this, you'll need Python and `uv`. Which you can install via `brew
      install uv`.
    - Then you'll have to set the transport to `stdio` in the apollo config and run `pnpm mcpo`.

## OpenWebUI

To add this MCP server to OpenWebUI, you can add the `mcpo` proxy address
(`http://localhost:8000` by default) to a new "Tool" entry. Found in the admin
portal under "Tools".

## Claude

To add this to Claude Desktop, you don't need the `mcpo` OpenAI proxy. Just add the following to your Claude Desktop config
(`~/.config/Claude/claude_desktop_config.json`) and you can connect directly to
the `apollo-mcp-server` URL.

```json
{
  "mcpServers": {
    "thespacedevs": {
        "command": "npx",
        "args": [
            "mcp-remote",
            "http://127.0.0.1:5001/mcp"
        ]
    }
  }
}
```

## Notes

- https://www.apollographql.com/blog/getting-started-with-apollo-mcp-server-for-any-graphql-api
- https://www.apollographql.com/docs/apollo-mcp-server
- https://modelcontextprotocol.io/legacy/tools/inspector#inspector
