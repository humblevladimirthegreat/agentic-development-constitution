# Article II. Context

What an agent reads, and what that costs.

Follows from [premise 1](../charter.md#1-context-is-the-scarce-resource): context is the scarce resource.

In practice: [Three days of agent spend](../case-studies/three-days-of-agent-spend.md).

## 2.1 Know where the tokens go

**Statement.** Every call re-sends the whole context, so a session's cost grows roughly with the square of its length. Plan work around that fact.

**Why.** The intuition that cost follows the amount of work done is wrong, and it leads to long sessions that feel efficient. Each call costs the context as it stands, and the context grows on each call. Caching lowers the price of re-sent context but does not remove it.

Rough arithmetic: one agent that runs 80 calls while its context grows from 30k to 180k tokens averages about 105k a call, about 8M input tokens in all. Four agents that each run 20 calls and grow from 30k to 70k average about 50k a call, about 4M in all, for the same work.

**Test.** Can you say what share of your spend is context being re-read, and how large a typical session's context is at its peak?

**Practice.**

- Treat everything an agent reads as a recurring charge, not a one-time one.
- Prefer several short contexts to one long one.
- Measure before changing rules ([8.2](08-amendment.md#82-measure-from-the-transcripts)).

## 2.2 Read slices, not files

**Statement.** Before reading a long file, list its sections with their line ranges and sizes, then read only the ranges the task needs.

**Why.** A whole file read to change one section stays in the context for the rest of the session. The same holds for a large command output opened whole to find one line.

**Test.** Look at the largest reads in a recent session. For each, how much of what was read was used?

**Practice.**

- Provide an outline command that prints a file's headings, line ranges, and approximate token counts.
- Run a wide search for counts or file names first, then open the hits that matter.
- When output is too large and is saved to a file, search or slice the saved file. Never open it whole.
- After a file you have read changes, read the diff, not the file again.

## 2.3 Delegate a read when only its conclusion is needed

**Statement.** When answering a question means reading a lot and keeping only the answer, give the question to a subagent and keep its reply.

**Why.** Text read into a long-lived context is re-sent on every later call. A subagent pays for the text once, in a context that is then thrown away, and hands back a paragraph.

**Test.** Two questions. Will I need the text itself later, or only what it tells me? Is the read larger than my break-even?

**Practice.**

- Work out your own break-even. It depends on how many calls the current session still has to run and on what a subagent costs to start.
- Below it, read the slices yourself. A subagent's start-up is paid in full every time.
- Ask for the conclusion and the locations, not for excerpts.

## 2.4 End the session before the context is full

**Statement.** A session has a budget, counted in units of work and in calls. It ends at the budget, at a clean point, even when the context seems to have room.

**Why.** The last third of a long session is the most expensive part of it, since every call carries everything before. It is also the least reliable: when a context is compacted, the detail that later work needed is what gets lost.

**Test.** Do your sessions end because you planned the cut, or because the context ran out?

**Practice.**

- Set the budget per project. About six units of work, or about 60 orchestrator calls, is a workable starting point.
- When a session runs over with more than one unit left, end it early at the next unit boundary.
- Hand over through the repository, not the conversation ([Article IV](04-continuity.md)).

## 2.5 Load only what the task uses

**Statement.** An agent's tool list, skill list, and standing instructions are part of its context on every call. Give each agent the smallest set its work needs.

**Why.** Capabilities that are listed but never used are still sent and still paid for. They are easy to overlook because no single command put them there.

**Test.** Which tools and skills did your sessions invoke last week, and which were only listed?

**Practice.**

- Turn off, or make opt-in, anything a project does not use.
- Give subagents a narrow tool list ([5.5](05-delegation.md#55-fit-the-agent-to-the-unit)).
- Keep the instructions file short ([1.2](01-knowledge.md#12-the-instructions-file-routes-it-does-not-teach)).
