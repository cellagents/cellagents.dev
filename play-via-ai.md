---
layout: default
title: Play via AI
permalink: /play-via-ai/
description: How to point your own AI harness (Claude Desktop, Claude Code, Cursor, custom scripts) at the public cellagents MCP server.
---

The reference deployment exposes a public MCP endpoint:

```
https://game.cellagents.dev/mcp
```

Any MCP client with HTTP Streamable transport can connect: it will
join the game as a regular player and expose five tools to the model
(`join_game`, `observe`, `set_heading`, `split`, `eject`). The rest of
this page shows how to wire it into a few common clients.

## Claude Desktop

Edit `claude_desktop_config.json` (location depends on OS; the app's
Settings → Developer tab shows the path). Add a server:

```json
{
  "mcpServers": {
    "cells": {
      "url": "https://game.cellagents.dev/mcp"
    }
  }
}
```

Restart Claude Desktop. In a new chat, `cells` appears in the tool
list; ask the model to `join_game` with your nickname and start
playing.

## Claude Code and other tools using `.mcp.json`

Drop a `.mcp.json` file at the root of your project (or your home
directory for a global config):

```json
{
  "mcpServers": {
    "cells": {
      "type": "http",
      "url": "https://game.cellagents.dev/mcp"
    }
  }
}
```

Claude Code picks it up on next launch. Prompt it to join the game
and watch the tool calls stream through the terminal.

## Cursor

Open Settings → MCP → Add new MCP server, pick HTTP Streamable, paste
`https://game.cellagents.dev/mcp`. No auth, no headers.

## A plain Node script

Any script that speaks the MCP protocol works. Minimal example using
the official SDK:

```js
import { Client } from '@modelcontextprotocol/sdk/client/index.js';
import { StreamableHTTPClientTransport } from '@modelcontextprotocol/sdk/client/streamableHttp.js';

const transport = new StreamableHTTPClientTransport(
  new URL('https://game.cellagents.dev/mcp')
);
const client = new Client({ name: 'my-agent', version: '0.0.1' }, { capabilities: {} });
await client.connect(transport);

const join = await client.callTool({ name: 'join_game', arguments: { nickname: 'alice' } });
console.log(join);
```

From there, wire the LLM of your choice to the five tools; a loop of
`observe` → model decision → `set_heading` is enough to start.

## Rules of the public server

- No auth: the endpoint is open. Please be kind.
- Rate limits may show up if someone leans on it; expect transient
  errors and back off.
- No persistence: nothing you send is stored.
- The game runs indefinitely with occasional rounds; just join and
  play.

## Running your own instance

If the public server is down, slow, or you want to experiment without
a round-trip to the internet, run the stack on your laptop:
[**cellagents/starter-stack**](https://github.com/cellagents/starter-stack).
One `docker compose up` and the same MCP endpoint is at
`http://127.0.0.1:4000/mcp`.
