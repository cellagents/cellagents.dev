---
layout: default
lang: en
title: Educators
permalink: /educators/
description: A one-shot classroom demonstration where humans, scripted bots and an LLM-driven agent share one game at the same time, so students can see the difference live. Audience, learning outcomes, lesson arc, and conditions.
translations:
  cs: /cs/pro-ucitele/
---

This page describes the pedagogical side of Cell agents, as a
one-shot school demonstration. Cell agents is an open educational
project where humans, scripted bots and LLM-driven agents play a
multiplayer game together. The entire project is free to edit and
extend to fit specific teaching needs.

The point is not that students program an AI during the lesson, but
that they **see three kinds of players share one world at the same
time** and can tell them apart by how they move.

## Audience and format

A school demonstration for students aged 12 to 15. The presentation
slot is 15 to 20 minutes, with about 25 participants, each on their
own laptop or phone with an internet connection. Students join the
game as human players. The teacher brings up the scripted bots and
the LLM-driven agent on the projector. The project remains available
as a public repository, so students can keep playing at home, modify
the code and deepen their understanding. The deployed game
environment remains running on cellagents.dev.

## The main idea

This project forks an open-source game `agar.io-clone`, a clone of
the popular browser game `agar.io`, where a cell moves around a map,
collects food, splits and devours smaller players. Whoever survives
and grows the most wins.

The game was chosen so it is publicly recognizable, works for a
large-group demonstration, and an open version exists that students
can experiment with later without licensing concerns. Familiarity
with the game is deliberate: students don't have to think about how
the game works and can focus entirely on **how different kinds of
players behave** in the same world, with the original game always
available for comparison.

### What the lesson deliberately does *not* teach

It's worth being explicit about what is **out of scope**, so
expectations are correct.

- The lesson doesn't teach how to train models. We use existing,
  third-party ones.
- It doesn't teach any specific programming language. The
  repositories are in TypeScript, but the point does not stand or
  fall on syntax.
- Students don't write prompts during this lesson. The LLM-driven
  agent runs with a prepared configuration so the demo stays within
  its slot. Prompt-writing is a natural follow-up at home.
- It is not a security course, though it touches on one principle of
  security (trust at the client-server boundary).

## What students take away

### The main mental model

**Almost anything existing in the software world can be wired up to
artificial intelligence and let a model drive it.** `agar.io` knows
nothing about AI. It wasn't written for AI. Yet we can control it
with a language model without changing its code significantly, and
in the same breath we can also point a scripted non-AI bot at it, or
play it ourselves. The server does not care which; it just sees
players. The same holds for email clients, calendars, editors,
payment systems, robots: anything with an interface.

This insight is more durable than any specific technology a student
might remember.

### Sub-concepts students will touch

1. **Three kinds of players, one game.** Humans, deterministic
   scripted bots and an LLM-driven agent share the same map at the
   same time. The game server sees them all as ordinary clients
   with a name. Side-by-side on the projector, their styles become
   recognisable: humans hesitate, bots are predictable, the LLM
   reacts to new situations but at a visibly different cadence.
2. **An LLM is an agent, not a chatbot.** The model doesn't just
   answer; it decides and acts in a loop. It picks what to do, does
   it, and observes the consequences.
3. **Scripted bots are a baseline.** Without AI, our bots still
   play, with hand-tuned rules. Seeing them in action makes it
   clearer what the LLM actually adds, and where it stumbles.
4. **Tools as the language between AI and the world.** The model
   has no hands. It has a list of functions it can call. What it
   can influence is exactly what we offer it.
5. **MCP (Model Context Protocol) as an industry standard.**
   Students see that there is an open protocol by which models are
   hooked up to tools, and that the same protocol is used by real
   products like Claude Desktop, Cursor, Zed. What we build here is
   not a toy or imitation. It is the same mechanism.
6. **A harness as the environment the model lives in.** A model by
   itself is just a function. Only when wrapped in a loop, memory
   and tools does something emerge that behaves like an agent. In
   class students see one running on the projector; at home they
   can run their own.
7. **Latency is not abstract.** In ordinary chat you don't notice
   the wait. But when a cell's survival depends on the answer,
   response time becomes a tangible strategic decision, especially
   next to a scripted bot that acts every tick.
8. **The trust boundary between client and server.** Some values
   the client computes itself and only reports to the server. The
   server must decide whether to believe the client. Students
   discover that a dishonest client can lie to the server, and that
   this is a general problem, not a quirk of our game.

### Lasting value

1. **Confidence to connect AI to existing systems.** Understanding
   that it is a surprisingly direct business opens up own
   experiments.
2. **A vocabulary.** Terms like tool, agent, harness, MCP, bot,
   trust boundary stop being foreign. The student meets them in
   media, documentation, tutorials and knows where to file them.
3. **A feel for what AI adds and what it doesn't.** Having watched a
   scripted bot and an LLM share one map, students have a concrete
   intuition for where a model earns its keep and where plain code
   is faster, cheaper and more reliable.
4. **The experience that programming AI is approachable.** The
   entire project has a clear README in each repo, comments in
   plain English and clear instructions for running or dismantling
   it at home. A student with minimal experience has a chance of
   understanding most.

## Lesson arc

### Before class

The teacher has already mentioned what a language model is, or
students themselves have experience with ChatGPT or similar.

### First 5 minutes: introduction

The teacher shows the game on the projector and names the three
players that will share this match:

- **Humans** (the students, in a moment).
- **Scripted bots**, hand-tuned, no AI inside.
- **An LLM-driven agent**, running through the harness, calling the
  game over MCP.

Two points are stressed:

- The game itself knows nothing about AI. AI is **hooked up from
  the outside**, through the same standard protocol used in real
  products.
- From the game's point of view all three look identical. It is a
  running race between a person, a script and a model.

### Middle 10 minutes: the match

Students open the link, enter a name and join. The scripted bots
are already in the game. The LLM-driven agent is brought up on the
projector so its tool-call log is visible to the room.

During the match the teacher narrates what is happening on three
layers:

- **How humans play.** Hesitation, risk, friends ganging up.
- **How the scripted bots play.** Clockwork pursuits, predictable
  flees, no surprises.
- **How the LLM plays.** The log on the projector shows each
  `observe → decide → act` cycle. Comments like: *"Notice the model
  paused for a full second here. The bot next to it moved three
  times in that gap. The model made the better long-term choice but
  paid for the latency."* The commentary turns the abstract
  principle into concrete events on screen.

Early phases are free: after death a player respawns smaller. Then
**sudden death** kicks in, where the next death is final. From a
full room a single survivor eventually remains, or a time limit
ends the match. Who wins (human, bot or LLM) matters less than
having felt each of them play.

### Last 5 minutes: debriefing

The teacher highlights the key points:

- The whole system is glue between a handful of independent
  components that talk standard protocols.
- The MCP protocol we used is the same one that, for example,
  large AI companies use to integrate models with tools in the
  world.
- Scripted bots and LLMs are not opposites; they are different
  tools for different jobs. Students have now seen both.
- Playing "fair" was our choice. The server does not check what
  the client reports. **This is the general principle of all APIs
  in the world.**
- At home anyone can download Claude Desktop, hook it up to our
  game server using the same protocol and play at full strength.
  The repository is public, instructions will be in the README.

## Practical conditions

- Each participant needs a device with a modern browser and an
  internet connection.
- The game server, the scripted bots and the LLM harness are run by
  the teacher or lesson organizer. Students install nothing.
- Model usage during the lesson is covered by the operator.
- After the lesson the server keeps running so students can play at
  home. The repository and instructions for running the stack
  independently remain available.

## Extensions, if time remains or as homework

- **Hooking up Claude Desktop or another client.** The student
  installs a ready-made AI client, adds our MCP server to its
  configuration, and plays directly from the chat.
- **Writing a prompt for the LLM agent.** The student runs the
  harness themselves and iterates on the strategy the model uses.
- **Tuning the scripted bots.** The bots expose a few personality
  knobs (aggressiveness, split willingness, cowardice). The student
  edits a config file and watches the match change.
- **Experimenting with cheating.** The student probes where the
  server trusts the client and how that can be exploited. This is a
  safe space; the game can take anything.
- **Changing the tools.** An advanced student adds a custom tool to
  the MCP server, for example a tool for communicating with other
  agents, and watches what happens.
- **New applications.** The most advanced student can try to apply
  the knowledge by building their own MCP server to control another
  game, program, website, or something reaching into the physical
  world.
