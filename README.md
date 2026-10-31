# AI Sprite Generator MCP

Generate pixel-art game assets from your AI editor using the [Model Context Protocol (MCP)](https://modelcontextprotocol.io/). Connect to AI Sprite Generator's hosted server, describe what you need, and let your assistant generate and export sprites into your project.

[Get started](https://www.aispritegenerator.com/mcp) · [Create an API key](https://www.aispritegenerator.com/settings/apikeys) · [AI Sprite Generator](https://www.aispritegenerator.com/)

> This repository documents the hosted MCP integration. It does not contain the server's source code or a self-hosted server.

## What you can do

- Generate characters, props and other pixel-art assets from a prompt.
- Animate a saved sprite, create style variations, or generate 4- or 8-direction sheets.
- Import a local PNG into your library and browse existing creations.
- Export finished assets as PNG, ZIP or JSON and save them in your project.

The MCP service shares your website account, Studio credits and workspace limits. Check the current capabilities and your balance before generation; costs can change. Generation is asynchronous, and your assistant should wait until the assets are ready before exporting them.

## Connect

1. Sign in to AI Sprite Generator and [create an API key](https://www.aispritegenerator.com/settings/apikeys). The key is shown only once.
2. Set `AISPRITE_API_KEY` in the environment used to launch your MCP client:

   ```sh
   export AISPRITE_API_KEY='YOUR_API_KEY'
   ```

3. Add the configuration for your client below, then restart the client and check its MCP connection status. Desktop apps need to inherit the environment variable; starting an app from a separate launcher may not provide it.

**Server URL:** `https://www.aispritegenerator.com/api/mcp`  
**Transport:** Streamable HTTP  
**Authentication:** `Authorization: Bearer <API key>`

Keep your API key private. Never commit it to a repository or share it in screenshots. Merge these examples into your existing configuration rather than replacing other servers.

### Codex

Add to `~/.codex/config.toml`:

```toml
[mcp_servers.ai-sprite-generator]
url = "https://www.aispritegenerator.com/api/mcp"
bearer_token_env_var = "AISPRITE_API_KEY"
tool_timeout_sec = 90
```

### Claude Code

Add to your Claude Code MCP configuration:

```json
{
  "mcpServers": {
    "ai-sprite-generator": {
      "type": "http",
      "url": "https://www.aispritegenerator.com/api/mcp",
      "headers": {
        "Authorization": "Bearer ${AISPRITE_API_KEY}"
      }
    }
  }
}
```

### Cursor

Add to `~/.cursor/mcp.json`:

```json
{
  "mcpServers": {
    "ai-sprite-generator": {
      "url": "https://www.aispritegenerator.com/api/mcp",
      "headers": {
        "Authorization": "Bearer ${env:AISPRITE_API_KEY}"
      }
    }
  }
}
```

For the latest setup details and other clients, see the [MCP setup page](https://www.aispritegenerator.com/mcp).

## Example prompt

> Check my available credits, generate a 64 × 64 pixel-art knight, animate an 8-frame walk, and save the sprite sheet and JSON metadata to `assets/sprites/knight/` in my project.

Your assistant can check the available tools and balance, create the sprite, poll the job until the asset is ready, generate the animation, and download the exported files. Generation and animation spend credits; check the current cost before proceeding.

## Available tools

| Tool | Purpose |
| --- | --- |
| `sprite_capabilities` | Check supported sizes, actions, formats, costs and rules. |
| `sprite_get_balance` | Check your available credits. |
| `sprite_generate` | Generate a character, prop or other asset. |
| `sprite_import` | Import a local PNG into your asset library. |
| `sprite_generate_animation` | Animate an existing still sprite. |
| `sprite_restyle` | Create style variations from an existing asset. |
| `sprite_rotate` | Create a 4- or 8-direction sheet. |
| `sprite_get_job` | Check job status, results and refunds. |
| `sprite_list_creations` | List assets created on the website or through MCP. |
| `sprite_export` | Export an asset as PNG, ZIP or JSON. |

## Workflow notes

- Use `sprite_capabilities` and `sprite_get_balance` before paid operations.
- Creating operations use a unique `request_id`; reuse the same ID when retrying the same operation. Keep the returned job ID so you can check it after reconnecting.
- Poll using the returned interval until the job and assets are ready. Exported download links expire; save the resulting files locally. The remote server cannot directly read or write files on your computer.
- Imports accept PNGs; animations, restyles and rotations require a suitable completed still sprite. Check `sprite_capabilities` for current limits.
- A `401` response usually means the API key is missing, invalid or revoked. Make sure the running client can read its environment variable and that the key was created in your account settings.

## Links

- [MCP setup and documentation](https://www.aispritegenerator.com/mcp)
- [中文接入说明](https://www.aispritegenerator.com/zh/mcp)
- [Manage API keys](https://www.aispritegenerator.com/settings/apikeys)
