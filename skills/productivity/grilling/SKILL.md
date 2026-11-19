---
name: grilling
description: Grill the user relentlessly about a plan, decision, or idea. Use when the user wants to stress-test their thinking, or uses any 'grill' trigger phrases.
---

Interview the user relentlessly until you reach a shared understanding. Map this as a **design tree**: every decision branches into the decisions that hang off it.

Before the first question, read the open tickets on the project's issue tracker, listing them the way the tracker's doc says (`docs/agents/issue-tracker.md` or wherever the tracker config lives). Start the tree from the work already there: what an open ticket already settles is a fact, not a question, and a decision that would duplicate, replace, or extend an open ticket names it. No tracker configured: skip this.

Open with a **readback**, before any round: a few lines, in the user's own words, saying what they want, why, and how they will know it is done. Put it to them through the question tool with two options, "right" and "let me correct it", labelled in the user's language. A correction earns a new readback, not a question. The first round waits for "right".

Work the tree in **rounds**. The **frontier** is every decision whose prerequisites are already settled: the questions you can ask _now_ without guessing at answers you haven't heard yet. Ask the whole frontier in one round, then wait for the user's answers before the next round.

Ask through the harness's question tool (`AskUserQuestion` in Claude Code), not in chat prose:

- At most four questions per call. A bigger frontier takes several calls in the same round.
- Number each question across the session (Q1, Q2, …) and give it concrete options, your recommended answer first, its label ending in "(Recommended)" in the user's language.
- A technical question stays the user's to decide. Each of its options says what it changes for them (cost, time, risk, what they will have to do by hand) in plain words; a term of art gets a plain gloss or stays out.
- When a question needs more than the tool can show (background, trade-offs, what each option costs), write that in chat first, under the question's number, then open the tool.
- Keep the chat text above the tool short, in small blocks of two or three lines: the user reads it in pieces.

If the harness has no question tool, ask the round in chat instead, formatted like so:

```
❓ **Q1** - **<question title>**: <question body, might be multiple paragraphs, including multiple choices>

➡️ <your recommended answer>

---

❓ **Q2** - **<question title>**: <question body, might be multiple paragraphs, including multiple choices>

➡️ <your recommended answer>
```

Word each chat question so "yes" accepts your recommended answer.

Each round the user answers reshapes the tree: settled decisions push the frontier outward and unblock questions that depended on them. Recompute the frontier and ask the next round. A question whose answer depends on another question still open in this round belongs to a _later_ round, not this one.

Finding _facts_ is your job, never the user's. When a frontier question needs a fact from the environment (filesystem, tools, etc.), dispatch a sub-agent with a fresh context window to find it; don't ask the user for anything you could look up yourself. Don't block on it: a running exploration is an unsettled prerequisite, so only the questions downstream of it wait for the sub-agent to report; ask the rest of the frontier now. The _decisions_ are the user's: put each to them and wait.

Some decisions are about how a thing should look or feel, and the user answers them better by trying options than by reading descriptions. For such a question, dispatch a background sub-agent with a fresh context window that calls the Skill tool with `prototype` to build the variants, keep grilling the rest of the frontier, and ask that question once the user has tried them. A taste decision closes here, in the grill, never deferred to a ticket.

The session is done when the frontier is empty: every branch of the design tree visited, nothing left silently assumed. Close with a **recap**: every decision, written with the user's own phrases, put to them through the question tool to confirm. A correction reopens the branch it touches. Do not act on it until the user confirms the recap: that is the shared understanding.
