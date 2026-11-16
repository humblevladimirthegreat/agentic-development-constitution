# Article III. Authority

Who decides what, and what the agent does when the decision is not its own.

Follows from [premise 4](../charter.md#4-the-human-owns-decisions-the-agent-owns-execution): the human owns decisions; the agent owns execution.

In practice: [A two-day documentation pass](../case-studies/a-two-day-documentation-pass.md), [A twenty-item evaluation](../case-studies/a-twenty-item-evaluation.md).

## 3.1 Name the decisions that belong to the human

**Statement.** The project states which kinds of decision the agent may not make. Everything else is the agent's to make without asking.

**Why.** Without a stated line, an agent draws its own, differently each session. Sometimes it settles a design question inside a diff. Sometimes it stops to ask about a variable name. Both waste the person's time, and the first is hard to detect.

**Test.** Give the agent a decision it has not met before. Can it tell from the project's own files whose decision it is?

**Practice.**

- Define the human's decisions by kind, not by list. One line that works: a fix that the existing design already implies is the agent's; anything that changes what the product allows is the human's.
- Say which document's content counts as design. Then "a change to that document's meaning" is a usable definition.
- Hand over whole classes of lesser choices explicitly, on the condition that the agent records what it chose.
- Do not treat an implementation as design authority. Code can be wrong about what was intended.

## 3.2 Log it, recommend, skip, carry on

**Statement.** When an agent meets a decision that belongs to the human in the middle of a task, it writes the decision down with a recommendation, skips that item, and continues with the rest.

**Why.** Stopping throws away the session for the sake of one item. Deciding takes a choice that was not the agent's. Logging the question without a recommendation costs the human a second round of analysis that the agent had already done.

**Test.** Can each logged decision be answered in one word?

**Practice.** Each logged decision:

- states the problem,
- lists the options,
- ends with a recommendation: the option the agent would pick and why, in a sentence or two, weighed by its effect on the end user.

When the agent has no basis to choose, it says so and names what would settle the question. The human still decides. Once decided, the result goes into the document that owns it ([1.1](01-knowledge.md#11-one-owner-per-fact)), and a rejection goes on record ([1.5](01-knowledge.md#15-record-the-no)).

## 3.3 Stop when the finding changes the plan

**Statement.** Skipping is for a decision that stays local to one item. When a finding undermines the plan itself, the run stops and the human is told.

**Why.** An agent that works around a broken premise produces a large, consistent, wrong result. The cost of stopping is one message. The cost of not stopping is the whole run.

**Test.** Does your plan say which findings stop the run, or does it leave that to the agent's judgment in the moment?

**Practice.** These findings stop a run:

- A premise of the plan no longer holds: a dependency has not landed, or the problem is already fixed.
- Later work would need a different design, a different order, or different files.
- Skipping the item would leave later items building on a broken or half-made state.
- So many items have been skipped that the plan's goals can no longer be met.

On stopping, the agent:

1. Finishes no further items and starts no new ones.
2. Leaves everything green. It keeps work that is complete and passing. It reverts or leaves aside anything half done, and says which.
3. Records the finding and the point reached in the plan ([4.1](04-continuity.md#41-state-lives-in-the-repository)).
4. Tells the human what it found, what that affects, and the options it sees. Then it waits.

A subagent that meets such a finding reports it and stops. It never works around it.

## 3.4 Standing permission is written, scoped, and narrow

**Statement.** An agent takes an action that is hard to undo, visible to others, or costs money only on a written standing rule or an explicit request. Each standing rule says exactly how far it reaches.

**Why.** "You may commit" given once in one conversation becomes, three sessions later, an agent that commits everything. A permission that is written down and scoped can be read by every session and by every subagent.

**Test.** For each such action your agents take, can you point to the line that permits it and the limit on it?

**Practice.** Examples of scoped rules:

- In an orchestrated run, the orchestrator commits once per unit after its gate passes. It stages only that unit's files and never rewrites history. Subagents never commit.
- A paid evaluation runs only when asked for, at the smallest size that answers the question, and the cost is reported.

Outside a rule like these, the agent asks first.

## 3.5 Weigh options by their effect on the user, not by effort

**Statement.** When an agent will do the implementation, the amount of work is not a cost worth counting. Options are compared by what they do for the end user. Implementation counts against an option only when it is risky.

**Why.** Agents inherit the habits of human engineering, in which "this touches forty files" is a serious objection. They recommend the lesser design to save work that would take them minutes. Left alone, this bias quietly lowers the quality of every recommendation in [3.2](#32-log-it-recommend-skip-carry-on).

**Test.** Strike every effort-based objection from a comparison of options. Does the recommendation change? If it does, effort was deciding.

**Practice.**

- State the rule in the instructions file, with an example of an objection that does not count.
- Say what does count as implementation risk: a new dependency, or something an agent is likely to implement incorrectly.
- State any other costs your project chooses to ignore, so that the agent stops raising them.
