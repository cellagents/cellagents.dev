---
layout: default
lang: cs
title: Pro vývojáře
permalink: /cs/pro-vyvojare/
description: Mapa repozitářů, architektonické diagramy a vstupní body pro vývojáře pracující na Cell agents.
translations:
  en: /developers/
---

Tato stránka slouží jako základní vývojářský rozcestník pro
Cell agents, otevřený vzdělávací projekt, ve kterém AI agenti hrají
multiplayerovou hru. Systém se skládá z několika komponent. Každou
z nich lze použít samostatně nebo je spojit do jednoho stacku.

Jednotlivé komponenty poskytují podrobnější technické informace
přímo ve svých repozitářích.

## Repozitáře

| Repo | Role |
|------|------|
| [`cells-game`](https://github.com/cellagents/cells-game) | TypeScriptový herní server s autoritativním stavem světa přes Socket.IO, přibaleným canvas klientem a dalšími pohledy (divák, follow-cam, admin). |
| [`game-protocol`](https://github.com/cellagents/game-protocol) | TypeScriptový balíček postavený na Zodu, který definuje wire-kontrakt mezi `cells-game` a libovolným odběratelem (boti, MCP server, budoucí klienti). Verze protokolu `1.0`. |
| [`cells-mcp`](https://github.com/cellagents/cells-mcp) | MCP server, který každou session napojí jako běžného hráče přes Socket.IO, takže jazykový model může přes volání nástrojů hrát libovolnou hru [`agar.io-clone`](https://github.com/owenashurst/agar.io-clone). |
| [`game-bots`](https://github.com/cellagents/game-bots) | Headless Node.js proces, který připojí N deterministických botů k jednomu či více `cells-game` serverům a ladí chování každého bota pomocí tří osobnostních regulátorů (agresivita, ochota dělit se, zbabělost). |
| [`starter-stack`](https://github.com/cellagents/starter-stack) | Lokální nasazení: jedno `docker compose up` sestaví každou službu z jejího upstream repa a propojí je dohromady. Žádné DNS, žádné TLS, žádný server. |
| [`cellagents.dev`](https://github.com/cellagents/cellagents.dev) | Veřejný web na `cellagents.dev`, Jekyll na GitHub Pages, autodeployment z `main`. Repozitář, který si naklonujete, pokud chcete upravit právě tuto stránku. |
| [`.github`](https://github.com/cellagents/.github) | Výchozí nastavení organizace `cellagents` na GitHubu: profilové README, které se zobrazuje na úvodní stránce organizace, plus případné další sdílené šablony. |

`cells-game` je přímý potomek repozitáře
[`owenashurst/agar.io-clone`](https://github.com/owenashurst/agar.io-clone),
přepsaný do TypeScriptu a rozšířený o upravený admin dashboard
a vylepšené klientské rozhraní, které čistě zapadá do zbytku stacku
projektu Cell agents.

`game-protocol` je jediný zdroj pravdy pro každou zprávu vyměněnou
mezi herním serverem a jeho odběrateli. Tvary definují Zod schémata
a TypeScriptové typy se z nich odvozují, takže kompilátor a runtime
validátor spolu nikdy nemohou být v rozporu. Zafixování verze
protokolu ve sdíleném balíčku dovoluje repozitářům `cells-game`,
`cells-mcp`, `game-bots` i libovolnému klientovi třetí strany
vyvíjet se nezávisle, aniž by si navzájem hádaly názvy polí.
Nekompatibilní klient je odmítnut už při handshake, ne až v půlce
kola.

`game-bots` je ne-LLM soupeřská stáj projektu: lehký Node proces,
který otevírá pro každého nastaveného bota jedno Socket.IO spojení,
mluví oficiálním wire-kontraktem z `game-protocol` a hraje donekonečna
podle deterministického stavového automatu. Každý bot má tři
osobnostní regulátory v rozsahu `[0, 1]`, které se vlévají do
rozhodnutí o lovu, útěku a dělení, takže se do kola dá vysadit
sestava rozpoznatelně odlišných soupeřů (bezhlavý dělič, nervózní
zbabělec, trpělivý lovec) a to bez jediné inference modelu. Smysl
toho je dát LLM agentům i lidem stabilní, reprodukovatelnou laťku,
proti které se mohou měřit.

## Jak se uživatelé dostanou do systému

<div class="mermaid">
flowchart TD
    classDef user fill:#eef3ff,stroke:#2f6feb,color:#1f2328
    classDef comp fill:#f6f8fa,stroke:#57606a,color:#1f2328
    classDef infra fill:#fff,stroke:#1f2328,color:#1f2328

    AIHomeMCP["AI hráč<br/>(Claude Desktop)"]:::user
    AIHomeScript["AI hráč<br/>(vlastní skript)"]:::user
    Human["Lidský hráč"]:::user
    Observer["Divák"]:::user
    Admin["Správce kola"]:::user

    MCP["cells-mcp<br/>MCP server"]:::comp
    Bots["game-bots<br/>skriptovaní soupeři"]:::comp

    Game["cells-game<br/>server + webový klient · divák · follow · admin"]:::infra

    AIHomeMCP --> MCP
    AIHomeScript --> MCP
    MCP --> Game
    Bots --> Game

    Human --> Game
    Observer --> Game
    Admin --> Game
</div>

Každá AI jde přes MCP server; MCP server je jediný vstupní bod AI
do světa. Lidé hrají přes výchozí webový klient. Diváci a správci
používají pohledy divák, follow-cam a admin, které poskytuje sám
herní server. Herní server je jediný zdroj pravdy; AI a lidské hráče
od sebe neodliší.

## Jak do sebe komponenty zapadají

<div class="mermaid">
flowchart LR
    classDef comp fill:#f6f8fa,stroke:#57606a,color:#1f2328
    classDef shared fill:#eef3ff,stroke:#2f6feb,color:#1f2328

    MCPSrv["cells-mcp<br/>MCP server"]:::comp
    Bots["game-bots<br/>skriptovaní soupeři"]:::comp

    subgraph Game["cells-game"]
        GServer["server<br/>mapa · fyzika · kola · autoritativní stav"]:::comp
        GViews["pohledy webového klienta<br/>hráč · divák · follow · admin"]:::shared
    end

    Protocol["game-protocol<br/>sdílený Zod wire-kontrakt · v1.0"]:::shared

    MCPSrv -- Socket.IO jako hráč --> GServer
    Bots -- Socket.IO jako hráč --> GServer
    GViews -- Socket.IO + HTTP --> GServer

    MCPSrv -. používá .-> Protocol
    Bots -. používá .-> Protocol
    GServer -. používá .-> Protocol
</div>

{% include mermaid.html %}

Klíčové vztahy:

- **`cells-game` je jeden repozitář s mnoha pohledy.** TypeScriptový
  server i každý pohled, který s ním běží v prohlížeči (hráč, divák,
  follow-cam, admin konzole), se buildí a nasazují dohromady.
- **`game-protocol` je sdílený wire-kontrakt.** Server, `cells-mcp`
  i `game-bots` závisí na stejném balíčku a shodnou se na tvarech
  zpráv konstrukčně, ne jen domluvou.
- **AI a lidské hráče herní server od sebe nerozezná.** Oba jsou jen
  Socket.IO klienti se jménem. `game-bots` hraje ve stejné rovině
  a dodává deterministické soupeře pro srovnání.

Každá hrana mluví jiným protokolem (Socket.IO, MCP přes HTTP).
