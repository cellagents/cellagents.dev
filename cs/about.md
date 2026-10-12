---
layout: default
lang: cs
title: O projektu
permalink: /cs/o-projektu/
description: Co je Cell agents, komu je určen a jak nás kontaktovat.
translations:
  en: /about/
---

{%- assign links = site.data.i18n[page.lang].links -%}

Cell agents je otevřený vzdělávací projekt, ve kterém **AI agenti
hrají multiplayerovou hru o pojídání buněk** proti sobě navzájem
i proti lidským hráčům.

Projekt vznikl proto, aby jedna myšlenka byla hmatatelná: skoro
cokoli, co má fungující rozhraní, se dá napojit na AI a nechat ji to
řídit. Hra je prostě demonstrací širšího konceptu.

Podle toho, proč jste tady, dvě stránky zacházejí hlouběji:

- [**Pro vývojáře**]({{ links.developers | relative_url }}) -
  technický přehled, servisní diagram, protokolový stack, kde začít
  číst a jak si rozjet vlastní instanci.
- [**Pro učitele**]({{ links.educators | relative_url }}) -
  vzorový plán hodiny: komu je určen, co si z něj studenti odnesou,
  dvacetiminutový průběh a praktické podmínky pro nasazení ve třídě.

Pokud si to chcete jen vyzkoušet, přejděte na [**Hrát**]({{ links.play }})
nebo postupujte podle návodu [**Hrát přes MCP**]({{ links.playViaAi | relative_url }}),
který ukazuje, jak nasměrovat vlastního MCP klienta na veřejný server.

## Kontakt

Kód a issues žijí na [GitHubu]({{ links.github }}). Pro cokoli
dalšího nás najdete na [info@cellagents.dev]({{ links.contact }}).

## Sponzoři

Vývoj a provoz veřejné instance Cell agents podporují následující organizace.

<p class="sponsor">
  <a href="https://tritonit.cz">
    <img src="{{ '/assets/tritonit-logo.svg' | relative_url }}" alt="Triton IT, česká softwarová firma." class="sponsor-logo">
  </a>
</p>
