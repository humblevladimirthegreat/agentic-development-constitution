# Article VIII. Amendment

How the rules change.

Follows from [premise 6](../charter.md#6-rules-are-measured-then-changed): rules are measured, then changed. This article governs the project's own rules for agents and this constitution alike.

In practice: [Three days of agent spend](../case-studies/three-days-of-agent-spend.md).

## 8.1 Every run ends by naming its waste

**Statement.** Each task, and each subagent's report, closes with a line or two naming anything in the run that looked inefficient: a large read, a rerun, a search that went wide.

**Why.** The agent is the only witness to most waste. It knows which read turned out to be useless and which check it ran twice. Asked at the end, it will say. Not asked, it will not, and the same waste recurs in the next session.

**Test.** Where do your ideas for better tooling come from: observation of runs, or guesses?

**Practice.**

- The agent notes the waste and moves on. It does not analyze it or propose a fix. Analysis at the end of every run would itself be waste.
- "None" is an acceptable answer.
- Read the lines in bulk now and then. A complaint that recurs is a candidate for a tool ([7.5](07-mechanization.md#75-prose-then-tool-then-hook)).

## 8.2 Measure from the transcripts

**Statement.** A report built from the sessions' own transcripts shows where the spend goes, and each change to the rules is checked against the numbers from before it.

**Why.** Rules about context are easy to write and easy to believe. Without measurement, a rule that saves nothing sits beside one that halves the bill, and both look equally wise.

**Test.** For the last rule you added to save cost, what was the number before and the number after?

**Practice.** The report covers a date window and prints:

- spend by component, weighted by price,
- the cost of carrying each kind of content from call to call,
- how often a check was rerun, or its log opened, after a failure,
- a table of subagents with their peak context and cost,
- which tools and skills were invoked.

Keep the last baseline where the report prints it, so that a new run is read against it.

## 8.3 A rule enters with a failure and leaves with a reason

**Statement.** A rule is added because a failure was observed or a measurement showed waste. It is removed explicitly, with the reason, when it no longer earns its cost.

**Why.** Rules that guard against imagined failures pile up without limit, and every one is paid for in every session. Rules also age. A caution written for last year's models may now do nothing but cost tokens.

**Test.** For any rule in your instructions file: what went wrong that put it there? Does that still go wrong without it?

**Practice.**

- When you add a rule, decide what measurement or observation would show that it helps, and look for it.
- When a model or a price changes, review the rules that rest on the premise affected ([charter](../charter.md)).
- A rule that is withdrawn is removed from where agents load it, and the reason is recorded. The record keeps it from being reinvented ([1.5](01-knowledge.md#15-record-the-no)).
- A caution about an action that is risky or cannot be undone stays until a person lifts it, even when its reason seems to have lapsed ([3.4](03-authority.md#34-standing-permission-is-written-scoped-and-narrow)).

## 8.4 A correction is saved where it will be loaded

**Statement.** When the human corrects the agent, the correction is written where the next session that needs it will read it, with its reason and its scope.

**Why.** A correction that lives in one conversation has to be given again in the next. One saved without its reason gets applied too widely or too narrowly, because the agent cannot tell what it was for.

**Test.** What was the last thing you told an agent twice?

**Practice.**

- Place it by reach. A rule every task needs goes in the instructions file. A step of one task goes in that task's procedure. A personal preference goes in the agent's memory, where the tool has one.
- Record what was said, when, and why, and label the agent's own interpretation as an interpretation.
- Save only what will change behavior in more than one future session. A one-time instruction is not a rule.
- Do not make a rule of a passing remark. When it is unclear whether something was an instruction, the agent asks once and carries on.
