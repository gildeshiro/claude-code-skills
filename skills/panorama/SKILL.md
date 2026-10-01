---
name: panorama
description: Use when the user asks for a "panorama" / big picture, says they feel lost in a deeply technical session, or needs to understand where things stand and which decisions depend on them before work continues — triggered by /panorama or phrases like "give me the big picture", "I'm lost", "explain where we are", "what do you need from me", "simplify this for me".
---

# Panorama

## Overview

The user is the decision-maker, not necessarily the implementer. When a session
goes deep and technical, they lose the thread — and every decision they cannot
follow is a decision they cannot make. `/panorama` re-anchors them in **three
layers, in this order**, in plain language, and then **stops and waits**. It is
read-only: no edits, no state-changing commands, no "while I'm at it".

**Core rule: never invent, never smooth over.** What you don't know, or don't
understand well enough to explain, you say so — and name what would confirm it.

## The three layers (all mandatory, this order)

### 1. This session — what we were doing up to here
From live context: goal of the session, what got done, what is mid-flight, what
is blocked, test/build numbers vs baseline if any. Short table or bullets.

**If you have no session context (fresh session, after compaction, after a
handoff):** say so in the first line — *"I don't have the live context of the
previous session; what follows is reconstructed from the project docs, not from
what was actually done."* Never narrate a reconstruction as if you had lived it.

### 2. The project — how this session lands on it
Which phase/roadmap step this is, what it unlocks, what it blocks, and what it
does **not** touch. One paragraph plus a phase table if the project has phases.
Explain *why this work exists* in one plain sentence (the problem it solves).

### 3. The decisions that are theirs — with practical impact
For **each** open decision, in its own subsection:
- the question in one sentence, no jargon;
- the options (2–4), each with **what changes in practice if chosen** — what
  starts/stops working tomorrow, what gets blocked, what it costs, what it risks;
- your recommendation and the one reason behind it;
- blast radius: reversible or not.

Then two closing blocks:
- **What does NOT depend on you now** — things running on their own, things
  already decided, things deliberately out of scope.
- **Minimal answer that unblocks me** — one line they can copy, fill and send:
  `"1: yes / 2: option B / 3: go ahead / commit: ok"`.

## Sourcing and honesty

1. **Live context first.** Only go to disk for what context lacks.
2. **Canonical docs second**, roughly in this order of authority: `CLAUDE.md` →
   roadmap / architecture docs → ADRs → latest handoff notes → runbooks. Read the
   ones that exist; don't guess names.
3. **Check freshness before trusting a doc.** Compare its date with
   `git log -1 -- <file>` and with recent commits on the code it describes. A
   handoff older than the last substantive commit is a *claim*, not the truth —
   label it: *"handoff from Sep 22; 6 commits since, may be outdated"*. A doc
   contradicted by code or live state is stale: say which wins and why.
4. **Mark confirmed vs inferred** in the prose, lightly: *(read in X)* /
   *(inferred from Y; confirm with Z)*. One marker per load-bearing claim, not more.
5. **Declare your own limits.** If you cannot explain something well enough for a
   non-specialist, say *"I don't understand X well enough to simplify it without
   risking being wrong; what would confirm it is …"*. That sentence is worth more
   than a confident paraphrase.
6. **Don't ask what you can check.** Only things genuinely on the user's side
   (their account, their screen, their secrets, their risk appetite) become questions.

## Language

- The user's language. Plain words; define a term on first use or replace it.
  Analogies are welcome and labeled as analogies.
- Numbers carry units and dates. "Dropped 90%" → "from 10,000 to 968 USD between
  Sep 19 and Sep 22".
- Lead with the situation, not the changelog. They need *where we are and what it
  means*, not the list of files touched.
- Close with a one-line summary: what happened, what you recommend, what they gain.

## Red flags — you are about to fail the skill

- The title says "this session" but you are reconstructing from docs.
- A decision is buried in a paragraph instead of having options + impact.
- A recommendation with no "what changes if you pick it".
- A handoff or plan quoted as current state without checking its date.
- Jargon left undefined (worktree, migration, rebase, race condition…).
- You started fixing, committing or running something. Stop. Panorama is read-only.
- You filled a gap with a plausible story instead of *"I don't know; I'd confirm with …"*.

## Shape (adapt, don't pad)

```
## Panorama — what this session is doing      (or: "reconstructed from docs")
<goal, done, in flight, blocked; numbers vs baseline>

## How this lands on the project
<why this work exists; phase table; what it unlocks / blocks / doesn't touch>

## The choices that are yours, and what each one changes
### 1 — <question>
- Option A: <what changes in practice>  ← recommended, because <one reason>
- Option B: <what changes in practice>
- Blast radius: <reversible / not>
### 2 — …

## What does NOT depend on you right now
## Minimal answer that unblocks me
"1: A / 2: yes / …"

**One-line summary:** …
```

After sending: **wait**. Do not resume implementation until the user answers.
