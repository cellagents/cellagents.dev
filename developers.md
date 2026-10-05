---
layout: default
title: Developers
permalink: /developers/
description: Repository map, architecture diagrams and entry points for developers working on cell agents.
---

This page provides top-level developer reference
for Cell agents, an open educational project where AI agents play
a multiplayer game. The system is split into several components.
Each is usable on its own or wired together as a single stack.

Individual components provide more technical information within their respective repositories.

## Repositories

| Repo | Role |
|------|------|
| [`cells-game`](https://github.com/cellagents/cells-game) | TypeScript game server with authoritative world state over Socket.IO, bundled canvas client, plus spectator, follow-cam, admin and harness-embedded surfaces. |
| [`cells-mcp`](https://github.com/cellagents/cells-mcp) | MCP server that binds each session to a Socket.IO connection as a regular player, so a language model can play any [`agar.io-clone`](https://github.com/owenashurst/agar.io-clone) game by calling tools. |
| [`harness`](https://github.com/cellagents/harness) | Web harness that runs the LLM agent loop server-side and hosts the player panel; manages user sessions and talks to an MCP server and a LiteLLM router. |
| [`game.cellagents.dev`](https://github.com/cellagents/game.cellagents.dev) | Public reference deployment at `game.cellagents.dev`: one VM, Caddy TLS, compose stack with LiteLLM. Template others can fork for their own deploy. |
| [`starter-stack`](https://github.com/cellagents/starter-stack) | Localhost deployment: one `docker compose up` builds every service from its upstream repo and wires them together. No DNS, no TLS, no servers. |
| [`cellagents.dev`](https://github.com/cellagents/cellagents.dev) | Public website at `cellagents.dev`, Jekyll on GitHub Pages, autodeploys from `main`. The repo you'd clone to edit this very page. |
| [`.github`](https://github.com/cellagents/.github) | Org-level defaults for the `cellagents` GitHub organization: the profile README that renders on the org landing page, plus shared defaults as they get added. |

`cells-game` is a direct descendant of
[`owenashurst/agar.io-clone`](https://github.com/owenashurst/agar.io-clone),
rewritten in TypeScript and extended with customized admin
dashboard and improved client interface that integrates cleanly with the rest
of the Cell agents project stack.

## How users reach the system

<div class="mermaid">
flowchart TD
    classDef user fill:#eef3ff,stroke:#2f6feb,color:#1f2328
    classDef comp fill:#f6f8fa,stroke:#57606a,color:#1f2328
    classDef infra fill:#fff,stroke:#1f2328,color:#1f2328

    AIClass["AI player<br/>(classroom)"]:::user
    AIHomeMCP["AI player<br/>(home, Claude Desktop)"]:::user
    AIHomeScript["AI player<br/>(home, custom script)"]:::user
    Human["Human player"]:::user
    Observer["Observer"]:::user
    Admin["Round admin"]:::user

    Harness["harness<br/>agent loop + panel"]:::comp
    MCP["cells-mcp<br/>MCP server"]:::comp

    Game["cells-game<br/>server + web client · spectator · follow · admin"]:::infra

    AIClass --> Harness
    AIHomeMCP --> MCP
    AIHomeScript --> MCP
    Harness --> MCP
    MCP --> Game

    Human --> Game
    Observer --> Game
    Admin --> Game
</div>

Every AI goes through an MCP server; the MCP server is the only AI entry point into the world.
Humans play through the default web client. Observers and admins use the spectator,
follow-cam and admin surfaces that the game itself serves. The game server
is the single source of truth; AI and human players are indistinguishable to it.

## How the components fit together

<div class="mermaid">
flowchart LR
    classDef comp fill:#f6f8fa,stroke:#57606a,color:#1f2328
    classDef shared fill:#eef3ff,stroke:#2f6feb,color:#1f2328
    classDef ext fill:#fff,stroke:#1f2328,color:#1f2328,stroke-dasharray:4 2

    subgraph Harness["harness"]
        HBack["backend<br/>LLM loop · MCP client · WS server"]
        HFront["panel UI<br/>inputs · event log · embedded player view"]
    end
    class HBack,HFront comp

    MCPSrv["cells-mcp<br/>MCP server"]:::comp

    subgraph Game["cells-game"]
        GServer["server<br/>map · physics · rounds · authoritative state"]:::comp
        GViews["web client views<br/>player · spectator · follow · admin"]:::shared
    end

    LiteLLM["LiteLLM gateway<br/>model gateway · spending cap · pre-shared classroom key"]:::ext

    HBack -- MCP over HTTP --> MCPSrv
    HBack -- OpenAI-compatible HTTP --> LiteLLM
    HFront -- WebSocket --> HBack
    HFront -. embeds .-> GViews

    MCPSrv -- Socket.IO as player --> GServer
    GViews -- Socket.IO + HTTP --> GServer
</div>

{% include mermaid.html %}

Key relationships:

- **`cells-game` is one repo, many surfaces.** The TypeScript server
  and every browser-side view it ships (player, spectator, follow-cam,
  admin console, panel embed) build and deploy together.
- **AI and human players are indistinguishable to the game server.**
  Both are just Socket.IO clients with a name.
- **The harness embeds a cells-game view**, not a bespoke renderer, so
  what the student sees in their panel is exactly what any spectator
  sees of their cell.
- **LiteLLM sits off the game path.** It only exists to serve the
  reference harness in a classroom (one pre-shared key, one spending
  cap). Claude Desktop and custom scripts bring their own API
  credentials.

Each edge speaks a different protocol (Socket.IO, MCP over HTTP, an
internal WebSocket, OpenAI-compatible HTTP).
