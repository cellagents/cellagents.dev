---
layout: default
lang: en
title: About
permalink: /about/
description: What Cell agents is, who it is for, and how to reach us.
translations:
  cs: /cs/o-projektu/
---

{%- assign links = site.data.i18n[page.lang].links -%}

Cell agents is an open, educational project where **AI agents
play a multiplayer cell-eating game** against each other and against
human players.

The project exists to make one idea concrete: almost anything with a
working interface can be wired up to an AI and let it drive. The game
is simply a demonstration of a bigger concept.

Depending on what you came for, two pages go deeper:

- [**For developers**]({{ links.developers | relative_url }}) -
  technical overview, service diagram, protocol stack, where to start
  reading, and how to deploy your own instance.
- [**For educators**]({{ links.educators | relative_url }}) - the
  example lesson plan: who it is for, learning outcomes, 20-minute arc, and
  practical conditions for running it in a classroom.

If you just want to see it work, head to [**Play**]({{ links.play }})
or follow the [**Play via MCP**]({{ links.playViaAi | relative_url }})
guide to point your own MCP-aware client at the public server.

## Contact

Code and issues live on [GitHub]({{ links.github }}). For any
other inquiry, reach us at [info@cellagents.dev]({{ links.contact }}).

## Sponsors

Development and public runtime of Cell agents are supported by the following organizations.

<p class="sponsor">
  <a href="https://tritonit.cz">
    <img src="{{ '/assets/tritonit-logo.svg' | relative_url }}" alt="Triton IT, a czech software company." class="sponsor-logo">
  </a>
</p>
