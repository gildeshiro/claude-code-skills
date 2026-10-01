---
name: session-handoff
description: Use when the user wants to end the current session and continue in a fresh one (context is getting long, compaction is near, or a side-topic deserves its own session) and asks for a copy-paste prompt — "give me a handoff prompt", "handoff in a code block", "write a prompt for the next session", "I want to start a new session with this context", "make a prompt so another agent can investigate this", /handoff-prompt.
---

# Session Handoff (prompt in a code block)

## Overview

Long sessions get heavy: the context fills up, auto-compaction loses detail, or a
side-topic deserves its own clean session. The user then pastes a prompt into a
**fresh session that knows nothing**. The handoff is that session's entire world:
anything not in the block does not exist for it.

**Core rule: self-contained, verified, decision-free.** The next session must be
able to act on its first turn without asking what was already answered here, and
without re-discovering what was already verified here.

## Process

1. **Scope the next session.** What is it for — finish open items, a dedicated
   topic, a side investigation in another repo? If the user named it, use their
   words. If items from here belong to *different* future sessions, sort them and
   say which go where (don't dump everything into one prompt).
2. **Resolve decisions NOW, before writing.** Any choice the next session would
   stall on (which approach, which spec, push or not, what's out of scope): ask
   the user *in this session* (AskUserQuestion if available). Record the answers
   in the handoff as settled decisions, quoting the user's wording.
3. **Capture hard state with evidence.** Run, don't recall: `git status`,
   `git log -1 --oneline`, current branch, local vs remote, worktrees, running
   servers/background jobs, test results vs baseline. Every file:line or symbol
   you cite — open it and confirm it still says that. Mark anything unverified as
   *inferred*.
4. **Mirror the session model.** If this session worked as orchestrator +
   subagents, plan-first, TDD, or any specific protocol, the handoff states the
   same model explicitly. If the user says "same format as the prompt you got at
   the start", reuse that prompt's structure section by section.
5. **Write the block** (template below). Then, outside the block, one short
   paragraph saying what was verified vs inferred, and offer to also save a copy
   to the project (e.g. `docs/handoffs/YYYY-MM-DD-<slug>.md`) — offer, don't write,
   unless the project already does this by convention.

## Template

Fence with **four backticks** (` ````markdown `) so inner ``` blocks survive the
copy-paste.

````markdown
# Handoff — <project>: <goal of next session> (<date>)

## Your role
<e.g. You are the orchestrator; delegate heavy reading and mechanical edits to
subagents, keep the decisions, verify what comes back.> <Reply language, if relevant.>

## Where things stand (verified <date/time>)
- Repo `<path>` — branch `<b>` @ `<hash>`; remote: <pushed / ahead N / dirty: files>
- Running: <servers, background jobs, worktrees> | Tests: <N pass / M fail {names}>
- Done this session: <bullets with commit hashes>

## Settled decisions (do not re-ask)
- <decision> — user: "<their words>"

## Goal & scope
<what to do; what is explicitly out of scope; items routed to other sessions>

## Open items (ordered)
1. <item> — evidence: <file:line / log / finding>; done when: <acceptance criteria>

## Read first
1. `CLAUDE.md`  2. `<relevant doc>` §N  3. <other context that applies>

## Constraints & gotchas
<safety rules, what not to touch, traps hit this session, flaky tests>

## First action
<the concrete first step, so turn 1 is productive>

## Report back
<what the closing report must contain: state, hashes, test delta, steps left for the user>
````

## Red flags — stop and fix

| Temptation | Reality |
|---|---|
| "Continue from where we left off" | The next session has no "where". Spell it out. |
| Citing file:line from memory | Re-open it. Stale pointers waste the new session's first hour. |
| Leaving "decide X" as an open item | Ask now; decisions belong before the next session starts. |
| Summarizing the whole chat | Only what the next session needs to act. History ≠ handoff. |
| Triple-backtick outer fence | Inner code blocks break the copy. Use four. |
| Secrets/tokens in the block | Reference where they live, never the value. |
| Omitting the working model | The next session defaults to solo work; state the model explicitly. |
