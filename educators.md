---
layout: default
title: Educators
permalink: /educators/
description: A one-shot classroom demonstration where students write prompts and an LLM plays a multiplayer game on their behalf. Audience, learning outcomes, lesson arc, and conditions.
---

This page describes the pedagogical side of Cell agents, as a
one-shot school demonstration. Cell agents is an open educational
project where AI agents play a multiplayer game. The entire project
is free to edit and extend to fit specific teaching needs.

Students play the game by configuring AI agent instructions in natural language. The language model plays on their behalf.

## Audience and format

A school demonstration for students aged 12 to 15. The
presentation slot is 15 to 20 minutes, with about 25 participants, each
on their own laptop or phone with an internet connection. The project
remains available as a public repository, so students can keep
playing at home, modify the code and deepen their understanding.
The deployed game environment remains running on cellagents.dev.

## The main idea

This project forks an open-source game `agar.io-clone`, a clone of the popular browser
game `agar.io`, where a cell moves around a map, collects food, splits
and devours smaller players. Whoever survives and grows the most wins.

The game was chosen so it is publicly recognizable, works for a
large-group demonstration, and an open version exists that students
can experiment with later without licensing concerns. Familiarity with
the game is deliberate: students don't have to think about how the
game works and can focus entirely on **how artificial intelligence
works in the role of a player**, with the original game always
available for comparison.

### What the lesson deliberately does *not* teach

It's worth being explicit about what is **out of scope**, so
expectations are correct.

- The lesson doesn't teach how to train models. We use existing,
  third-party ones.
- It doesn't teach any specific programming language. The repositories
  are in TypeScript, but the point does not stand or fall on syntax.
- It is not a prompt-engineering competition. Winning the match is a
  fun incentive, not the goal.
- It is not a security course, though it touches on one principle of
  security (trust at the client-server boundary).

## What students take away

### The main mental model

**Almost anything existing in the software world can be wired up to
artificial intelligence and let a model drive it.** `agar.io` knows
nothing about AI. It wasn't written for AI. Yet we can control it
with a language model without changing its code significantly. The
same holds for email clients, calendars, editors, payment systems,
robots - anything with an interface.

This insight is more durable than any specific technology a student
might remember.

### Sub-concepts students will touch

1. **An LLM is an agent. Demonstration of the difference from a
   chatbot.** The model doesn't just answer; it decides and acts. It
   picks what to do, does it, and observes the consequences.
2. **Tools as the language between AI and the world.** The model has
   no hands. It has a list of functions it can call. What it can
   influence is exactly what we offer it.
3. **MCP (Model Context Protocol) as an industry standard.** Students
   see that there is an open protocol by which models are hooked up to
   tools, and that the same protocol is used by real products like
   Claude Desktop, Cursor, Zed. What we build at school is not a toy
   or imitation. It is the same mechanism.
4. **A harness as the environment the model lives in.** A model by
   itself is just a function. Only when wrapped in a loop, memory and
   tools does something emerge that behaves like an agent. In class
   students use a very limited harness, but they realize that at home
   they can run a much stronger one.
5. **Latency is not abstract.** In ordinary chat you don't notice
   the wait. But when a game cell's survival depends on the answer,
   response time and model choice become tangible strategic
   decisions.
6. **Perception shaping: how a model sees the world.** The model
   receives only what `observe()` hands it. Students notice that
   changing what the model sees changes how it plays. It is a first
   step toward understanding why the input to a model is as important
   as the model itself.
7. **The trust boundary between client and server.** Some values the
   client computes itself and only reports to the server. The server
   must decide whether to believe the client. Students discover that a
   dishonest client can lie to the server, and that this is a general
   problem, not a quirk of our game.
8. **Fair play versus optimization.** Our classroom panel is
   deliberately weakened. At home each student can build a better,
   smarter, possibly even cheating agent. This tension is a didactic
   tool, not a bug.

### Lasting value

1. **Confidence to connect AI to existing systems.** Understanding
   that it is a surprisingly direct business opens up own experiments.
2. **A vocabulary.** Terms like tool, agent, harness, MCP, latency,
   trust boundary stop being foreign. The student meets them in
   media, documentation, tutorials and knows where to file them.
3. **The felt tension between fairness and optimization.** A life
   skill, not just a programming one. How much may I lean on a
   stronger tool than my opponent? What does fair play mean when the
   other side cheats?
4. **The experience that programming AI is approachable.** The entire
   project has a clear README in the repo, comments in plain English
   and clear instructions for running or dismantling it at home. A
   student with minimal experience has a chance of understanding
   most.

## Lesson arc

### Before class

The teacher has already mentioned what a language model is, or
students themselves have experience with ChatGPT or similar.

### First 5 minutes: introduction

The teacher shows the game on the projector. Explains briefly that AI
will drive the cell and the student's task is to write the strategy
by which the AI plays. Two points are stressed:

- The game itself knows nothing about AI. AI is **hooked up from the
  outside**, through a standard protocol used in real products.
- The student's control panel is deliberately limited. At home anyone
  can wire up their own agent and play better.

### Middle 10 minutes: the match

Students open the link, enter a name, receive a pre-filled access key
to the model, write their own strategy, pick a decision speed and a
model. The match begins.

During the match the teacher narrates what is happening on the
projector. Shows the live tool-call log of selected players. Comments
like: *"Notice that Alice let her agent think only once every five
seconds, to keep moves predictable. Right now Peter, whose agent
reacted faster, ate her."* The commentary turns the abstract
principle into concrete events on screen.

Early phases are free: after death a player respawns smaller. Then
**sudden death** kicks in, where the next death is final. From a full
room a single survivor eventually remains, or a time limit ends the
match.

### Last 5 minutes: debriefing

The teacher highlights the key points:

- The whole system is glue between four independent components that
  talk standard protocols.
- The MCP protocol we used is the same one that, for example, large
  AI companies use to integrate models with tools in the world.
- Playing "fair" was our choice. The server does not check what the
  client reports. **This is the general principle of all APIs in the
  world.**
- At home anyone can download Claude Desktop, hook it up to our game
  server using the same protocol and play at full strength. The
  repository is public, instructions will be in the README.

## Practical conditions

- Each participant needs a device with a modern browser and an
  internet connection.
- The game server, MCP server and model gateway are run by the teacher
  or lesson organizer. Students install nothing.
- Model usage during the lesson is covered by the operator.
- After the lesson the server keeps running so students can play at
  home. The repository and instructions for running the stack
  independently remain available.

## Extensions, if time remains or as homework

- **A custom harness.** The student writes an agent at home that can
  do more over the same protocol, for example remembering the course
  of a match and reacting to opponent patterns.
- **Hooking up Claude Desktop or another client.** The student
  installs a ready-made AI client, adds our MCP server to its
  configuration, and plays directly from the chat.
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
