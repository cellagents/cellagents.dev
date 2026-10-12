---
layout: default
lang: en
title: Developers
permalink: /developers/
description: Repository map, architecture diagrams and entry points for developers working on cell agents.
translations:
  cs: /cs/pro-vyvojare/
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
| [`game-protocol`](https://github.com/cellagents/game-protocol) | Zod-first TypeScript package that defines the wire contract between `cells-game` and any subscriber (bots, MCP server, future clients). Protocol version `1.0`. |
| [`cells-mcp`](https://github.com/cellagents/cells-mcp) | MCP server that binds each session to a Socket.IO connection as a regular player, so a language model can play any [`agar.io-clone`](https://github.com/owenashurst/agar.io-clone) game by calling tools. |
| [`game-bots`](https://github.com/cellagents/game-bots) | Headless Node.js process that connects N deterministic bots to one or more `cells-game` servers and tunes each bot's behaviour from three personality knobs (aggressiveness, split willingness, cowardice). |
| [`harness`](https://github.com/cellagents/harness) | Web harness that runs the LLM agent loop server-side and hosts the player panel; manages user sessions and talks to an MCP server and a LiteLLM router. |
| [`starter-stack`](https://github.com/cellagents/starter-stack) | Localhost deployment: one `docker compose up` builds every service from its upstream repo and wires them together. No DNS, no TLS, no servers. |
| [`cellagents.dev`](https://github.com/cellagents/cellagents.dev) | Public website at `cellagents.dev`, Jekyll on GitHub Pages, autodeploys from `main`. The repo you'd clone to edit this very page. |
| [`.github`](https://github.com/cellagents/.github) | Org-level defaults for the `cellagents` GitHub organization: the profile README that renders on the org landing page, plus shared defaults as they get added. |

`cells-game` is a direct descendant of
[`owenashurst/agar.io-clone`](https://github.com/owenashurst/agar.io-clone),
rewritten in TypeScript and extended with customized admin
dashboard and improved client interface that integrates cleanly with the rest
of the Cell agents project stack.

`game-protocol` is the single source of truth for every message
exchanged between the game server and its subscribers. Zod schemas
define the shapes and the TypeScript types are inferred from them,
so the compiler and the runtime validator can never disagree. Pinning
a protocol version in a shared package lets `cells-game`, `cells-mcp`,
`game-bots` and any third-party client evolve independently without
guessing at each other's field names, and lets us reject incompatible
clients at the handshake rather than halfway through a round.

`game-bots` is the project's non-LLM opponent pool: a lightweight
Node process that opens one Socket.IO connection per configured
bot, speaks the official `game-protocol` wire, and plays
indefinitely from a deterministic state machine. Each bot is
described by three personality knobs in `[0, 1]` which blend into
hunt, flee and split decisions, so a round can be seeded with a
cast of recognisably different opponents (a reckless splitter, a
nervous coward, a patient hunter) without any model inference. It
exists to give LLM agents and humans a stable, reproducible
baseline to measure themselves against.

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
    Bots["game-bots<br/>scripted opponents"]:::comp

    Game["cells-game<br/>server + web client · spectator · follow · admin"]:::infra

    AIClass --> Harness
    AIHomeMCP --> MCP
    AIHomeScript --> MCP
    Harness --> MCP
    MCP --> Game
    Bots --> Game

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
    Bots["game-bots<br/>scripted opponents"]:::comp

    subgraph Game["cells-game"]
        GServer["server<br/>map · physics · rounds · authoritative state"]:::comp
        GViews["web client views<br/>player · spectator · follow · admin"]:::shared
    end

    Protocol["game-protocol<br/>shared Zod wire contract · v1.0"]:::shared
    LiteLLM["LiteLLM gateway<br/>model gateway · spending cap · pre-shared classroom key"]:::ext

    HBack -- MCP over HTTP --> MCPSrv
    HBack -- OpenAI-compatible HTTP --> LiteLLM
    HFront -- WebSocket --> HBack
    HFront -. embeds .-> GViews

    MCPSrv -- Socket.IO as player --> GServer
    Bots -- Socket.IO as player --> GServer
    GViews -- Socket.IO + HTTP --> GServer

    MCPSrv -. uses .-> Protocol
    Bots -. uses .-> Protocol
    GServer -. uses .-> Protocol
</div>

{% include mermaid.html %}

Key relationships:

- **`cells-game` is one repo, many surfaces.** The TypeScript server
  and every browser-side view it ships (player, spectator, follow-cam,
  admin console, panel embed) build and deploy together.
- **`game-protocol` is the shared wire contract.** The server,
  `cells-mcp` and `game-bots` all depend on the same package and
  agree on message shapes by construction, not by convention.
- **AI and human players are indistinguishable to the game server.**
  Both are just Socket.IO clients with a name. `game-bots` lives in
  the same lane and provides deterministic opponents for comparison.
- **The harness embeds a cells-game view**, not a bespoke renderer, so
  what the student sees in their panel is exactly what any spectator
  sees of their cell.
- **LiteLLM sits off the game path.** It only exists to serve the
  reference harness in a classroom (one pre-shared key, one spending
  cap). Claude Desktop and custom scripts bring their own API
  credentials.

Each edge speaks a different protocol (Socket.IO, MCP over HTTP, an
internal WebSocket, OpenAI-compatible HTTP).
