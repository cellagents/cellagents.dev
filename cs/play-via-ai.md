---
layout: default
lang: cs
title: Hrát přes MCP
permalink: /cs/hrat-pres-ai/
description: Jak nasměrovat vlastní AI harness (Claude Desktop, Claude Code, Cursor, vlastní skripty) na veřejný MCP server Cell agents.
translations:
  en: /play-via-ai/
---

**Neznáte MCP?** MCP (Model Context Protocol) je drobný standardní
způsob, jak AI asistent typu Claude mluví s vnějšími nástroji
a službami. Představte si ho jako univerzální zástrčku: AI zůstává
stejná a cokoli, co MCP ovládá, databáze, hra, váš kalendář, se
najednou stane něčím, co AI vidí a dokáže v tom jednat. V tomhle
projektu je herní server sám MCP serverem, takže libovolný
MCP-ready asistent se připojí a hraje bez jediného řádku vlastní
integrace.

Referenční nasazení vystavuje veřejný MCP endpoint:

```
https://game.cellagents.dev/mcp
```

Libovolný MCP klient s HTTP Streamable transportem se umí připojit:
připojí se do hry jako běžný hráč a modelu zpřístupní pět nástrojů
(`join_game`, `observe`, `set_heading`, `split`, `eject`). Zbytek
této stránky ukazuje, jak ho napojit v několika běžných klientech.

## Claude Desktop

Upravte `claude_desktop_config.json` (umístění záleží na OS; cestu
ukáže záložka Settings → Developer v aplikaci). Přidejte server:

```json
{
  "mcpServers": {
    "cells": {
      "url": "https://game.cellagents.dev/mcp"
    }
  }
}
```

Restartujte Claude Desktop. V novém chatu se `cells` objeví mezi
nástroji; požádejte model, aby zavolal `join_game` s vaší přezdívkou
a začněte hrát.

## Claude Code a další nástroje využívající `.mcp.json`

Vložte soubor `.mcp.json` do kořene projektu (nebo do domovského
adresáře pro globální konfiguraci):

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

Claude Code ho vyzvedne při příštím spuštění. Řekněte mu, aby se
připojil do hry, a volání nástrojů poteče terminálem.

## Cursor

Otevřete Settings → MCP → Add new MCP server, vyberte HTTP
Streamable a vložte `https://game.cellagents.dev/mcp`. Žádná
autentizace, žádné hlavičky.

## Prostý Node skript

Funguje libovolný skript, který mluví MCP protokolem. Minimální
příklad s oficiálním SDK:

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

Odsud už stačí napojit vybraný LLM na pět nástrojů; smyčka
`observe` → rozhodnutí modelu → `set_heading` stačí k rozjezdu.

## Pravidla veřejného serveru

- Bez autentizace: endpoint je otevřený. Buďte prosím ohleduplní.
- Mohou se objevit rate limity, pokud ho někdo začne tahat víc, než
  má; počítejte s občasnými chybami a backoff.
- Žádná perzistence: nic z toho, co pošlete, se neukládá.
- Hra běží donekonečna s příležitostnými koly; prostě se připojte
  a hrajte.

## Běh vlastní instance

Pokud veřejný server neběží, je pomalý, nebo chcete experimentovat
bez cesty na internet, pusťte si stack u sebe na notebooku:
[**cellagents/starter-stack**](https://github.com/cellagents/starter-stack).
Jedno `docker compose up` a stejný MCP endpoint máte na
`http://127.0.0.1:4000/mcp`.
