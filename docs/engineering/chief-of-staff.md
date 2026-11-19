## What it does

`chief-of-staff` turns one long [session](https://www.aihero.dev/ai-coding-dictionary/session) into the person who carries a goal forward through the tickets on your issue tracker. It does the work through [subagents](https://www.aihero.dev/ai-coding-dictionary/subagent), each started with a fresh [context window](https://www.aihero.dev/ai-coding-dictionary/context-window), and keeps its own window for deciding what comes next.

It never starts work cold. Before anything else it runs a **startup check** on the tracker: what's open and blocked, what git shows is already done, which tickets duplicate each other or have no clear state, and what order to go in. It also flags the tracker's debt: ideas that never started and have sat idle for two weeks, steps waiting on you that sit outside the ticket for the machine they happen on or that an agent could do, and chains stalled on a choice of yours. Every operation on tickets comes from your tracker doc, so the same skill works on a GitHub tracker and on local ticket files.

## When to reach for it

You invoke this by typing `/chief-of-staff`, and the agent won't reach for it on its own.

Reach for it when the tracker holds more work than one sitting and you want one session to keep it moving: pick the next ticket, dispatch it, close what's done, ask you only what's yours to decide. For a single spec you just want built, use [implement-spec](https://aihero.dev/skills-implement-spec) instead.

## Prerequisites

A configured issue tracker, from [setup-matt-pocock-skills](https://aihero.dev/skills-setup-matt-pocock-skills). The chief has no tracker commands of its own.

## The startup check

The steps of the check are in the skill's `SKILL.md`; what matters to you is that it is a light pass, and a deep reorder of the whole tracker happens only when you ask.

The debt it flags comes back as one list among its questions, each ticket with a proposed move:

| Debt | Proposed move |
| --- | --- |
| An idea that never started, idle for two weeks and outside any spec in progress | Close it |
| A step of yours outside the ticket for the machine it happens on, or one an agent could do | Move it into that ticket, or let an agent do it |
| A chain stalled on your choice | Make the choice now |

You decide each move; it makes none of them alone.

## What it does alone, and what it asks

Bookkeeping it does alone and lists for you with links: closing done work and duplicates, merging steps that share a session, fixing labels and blocking edges, opening a ticket for a leftover or a real breakage. Anything that changes what a ticket delivers, closes your request as wontfix, files its own ideas, makes a debt move, or touches a skill or the [harness](https://www.aihero.dev/ai-coding-dictionary/harness) waits for your yes.

Work in progress lives on the tickets themselves: a claim, a comment when work starts, a comment with the commit when it ends, and your answers on the ticket they concern. After a [compaction](https://www.aihero.dev/ai-coding-dictionary/compaction) the chief rereads the tickets and carries on.

## Common questions

**Why doesn't it just close the tickets that went stale?**
Closing ideas, moving your steps and settling a stalled choice are bulk moves on your requests, so they wait for your yes. The check only surfaces them, with a proposal for each.

**Can it write the spec and tickets for a big new piece of work?**
No. `to-spec` and `to-tickets` are user-invoked, so no skill can run them. The chief tells you there's a large chunk of work and gives you the `/grill-with-docs` command to run in a fresh session. The tickets that come out of it show up at its next startup check.

## It's working if

- Its first message is the startup check, not a question about what to do.
- No subagent rebuilds a ticket that was already done on a branch.
- Stale ideas, stray steps of yours and stalled chains reach you as questions with a proposal, never as tickets closed behind your back.
- Every ticket it touched alone appears in its report with a link.
- Claimed tickets carry a start comment and an end comment with a commit.
- Nothing about a skill or your config changed without your yes.

## Where it fits

A **long-running standalone** that sits on top of the main flow: it drives [implement-spec](https://aihero.dev/skills-implement-spec) for ready tickets and [retro](https://aihero.dev/skills-retro) for its subagents, and sends large new work back to [grill-with-docs](https://aihero.dev/skills-grill-with-docs). For the whole map, see [ask-matt](https://aihero.dev/skills-ask-matt).
