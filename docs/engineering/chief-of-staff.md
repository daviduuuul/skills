## What it does

`chief-of-staff` turns one long [session](https://www.aihero.dev/ai-coding-dictionary/session) into the person who carries a goal forward. It does the work through [subagents](https://www.aihero.dev/ai-coding-dictionary/subagent), each started with a fresh [context window](https://www.aihero.dev/ai-coding-dictionary/context-window), and keeps its own window for deciding what comes next. For a large chunk of work it writes the spec and the tickets itself, then builds them with [implement-spec](https://aihero.dev/skills-implement-spec). It keeps what it learns in a shared wiki (here, the home's memory) and improves the agents' environment as it goes, with [retro](https://aihero.dev/skills-retro) on about one subagent session in five.

## When to reach for it

You invoke this by typing `/chief-of-staff`, and the agent won't reach for it on its own.

Reach for it when you want to hand over a goal and let one session pursue it on its own for hours. When you'd rather shape the work yourself, the main flow is yours: [grill-with-docs](https://aihero.dev/skills-grill-with-docs), then `to-spec`, `to-tickets` and `implement`.

## Common questions

**Does it keep its context for hours?**
That's the design: the code and the digging happen in subagents, and the tracker and the wiki carry what outlasts a compaction.

**Does it change things without asking?**
It improves the environment on its own, keeping only the changes the sessions' evidence backs.

## It's working if

- Every piece of work runs in a subagent, and its own window stays on decisions.
- Large work arrives on the tracker as a spec with tickets, built on one integration branch.
- What it learns lands in the wiki, dated.

## Where it fits

A **long-running standalone** that sits on top of the main flow and runs it end to end. For the whole map, see [ask-matt](https://aihero.dev/skills-ask-matt).
