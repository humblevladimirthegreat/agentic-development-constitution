# Article V. Delegation

When to hand work to a subagent, what to give it, and what to ask back.

Follows from [premise 1](../charter.md#1-context-is-the-scarce-resource): context is the scarce resource. A subagent is a way to spend context somewhere other than in the session that has to last.

In practice: [Three days of agent spend](../case-studies/three-days-of-agent-spend.md), [A two-day documentation pass](../case-studies/a-two-day-documentation-pass.md).

## 5.1 Delegate to buy a fresh context

**Statement.** Use a subagent when a fresh context buys something: work in clusters that share little, a session that would otherwise overflow, or a step that must not see what came before. Otherwise do the work in the session you have.

**Why.** A subagent is not free labor. It pays its start-up in full: the instructions file, its brief, and every procedure its task names, all before its first edit. For a few edits on one file, that costs more than doing them directly.

**Test.** For each delegated unit: what did the fresh context buy? If the answer is "nothing, it just felt like a separate task", the orchestrator should have done it.

**Practice.**

- The orchestrator does a small unit itself: a few edits on one file it can read in one slice.
- Running subagents in parallel saves time, not tokens. Each pays its own start-up.
- Prefer a fresh subagent to a copy of the orchestrator's conversation. By the middle of a run, the copy is larger than a brief.

## 5.2 The brief is a file, complete and short

**Statement.** Each subagent gets its task as a file that holds everything it needs to start. The prompt only names the file.

**Why.** Whatever the orchestrator writes into a prompt stays in the orchestrator's context for the rest of the session. A file is read once, by the subagent. An incomplete brief is paid for differently: the subagent explores to fill the gap, or reads the whole plan.

**Test.** Could the subagent begin work after reading only its brief and the files the brief names?

**Practice.** The brief carries:

- the item's own text, copied from the plan,
- the check that settles it, and any decision already made about it,
- a digest of the rules that apply, at most about 40 lines,
- links to the full rules, for when the digest does not settle a question,
- the files it may edit, and the report it must return.

It does not tell the subagent to read the whole plan. A plan that runs through subagents keeps one reusable digest. Briefs are built from parts by a script, and a finished brief is never printed back into the orchestrator's context.

## 5.3 Subagents are short-lived and stop themselves

**Statement.** A subagent gets one or two items, sized by how much it must read. It checks its own progress at a fixed point and stops cleanly if a lot is left.

**Why.** A subagent that runs long costs what [2.1](02-context.md#21-know-where-the-tokens-go) says any long context costs. Nothing outside a running subagent can make it stop, so the instruction to stop has to travel with it.

**Test.** What is the longest subagent run in your last project, in calls? Was that length planned?

**Practice.**

- Size a unit by the files it reads, not by its item count. Two items that read the same files may share a subagent. An item that needs new files gets a new one.
- Put a standing line in every brief. For example:

  > At about 25 tool calls, check what is left. If it is small, finish the item. If a significant amount is left, stop at a clean point, leave everything green, and report what is done, what is left as steps, and the files touched.

- The orchestrator starts a fresh subagent for what is left.

## 5.4 A report holds only what the orchestrator acts on

**Statement.** A subagent's report has fixed fields and a word limit. It never narrates the work.

**Why.** Every later call the orchestrator makes pays for the report again. The diff and the version history already hold what was edited. The orchestrator needs only what it must do next.

**Test.** Read a report and delete each sentence the orchestrator did not act on. How much is left?

**Practice.** The report's fields, in about 150 words:

- Done: one line.
- Choices made under delegated authority ([3.1](03-authority.md#31-name-the-decisions-that-belong-to-the-human)).
- Files touched, as a count.
- Changes that shared files need ([5.7](#57-one-writer-per-shared-file)).
- The result of the check.
- Skipped: the item, the reason, and any open decision with its recommendation ([3.2](03-authority.md#32-log-it-recommend-skip-carry-on)).
- Left: steps not done, if it stopped early.
- Inefficient: anything in the run that looked wasteful ([8.1](08-amendment.md#81-every-run-ends-by-naming-its-waste)).

Names or a worklist that a later unit will be briefed from may run past the limit. The orchestrator reads the report and a summary of the diff. It opens a full diff only when the report flags something, a check fails, or a file outside the unit's list changed.

## 5.5 Fit the agent to the unit

**Statement.** Each unit's agent type, model, and effort level are named in the plan, and each agent type carries only the tools its work uses.

**Why.** A general-purpose agent carries every tool the session has and pays for them on every call. A choice of model left to the moment of launch tends to default to the most expensive one, or to whatever the configuration happens to supply.

**Test.** Does the plan say, for each unit, which model runs it? Could a mechanical unit have run on a smaller one?

**Practice.**

- Define a narrow worker for plan units. Four tools are enough for most of them: run a command, read, edit, write.
- Mechanical steps, such as scripted renames and link fixes, may run on a smaller model at low effort. Judgment, prose, and audits stay on the strongest.
- Add a specialized agent type only when several units share a standing brief that the generic one lacks.

## 5.6 Review with eyes that did not write

**Statement.** An audit or review runs in a context that made none of the changes. Where a writer must not know something, the rule says what it may read.

**Why.** An agent that reviews its own work reads what it meant, not what it wrote. And some work is only valid when it is produced without certain knowledge: a test written from the specification is worth more than one written by an agent that has just read the implementation.

**Test.** For your last review step, what was in the reviewer's context that came from the author?

**Practice.**

- The last unit of a plan that changes prose or behavior is a review by a fresh subagent.
- Scope the review to what the plan changed, plus close neighbors. A change to one section can make the next one stale.
- Where a rule limits what an agent may have read, start that agent fresh and state the limit in its definition.

## 5.7 One writer per shared file

**Statement.** A file that many units touch (an index, a registry, the instructions file) has one writer at a time. Subagents report the changes it needs, and the orchestrator applies them.

**Why.** Two agents editing one registry at once overwrite each other, and neither notices.

**Test.** List the files that more than one unit of your plan edits. Is there a named writer for each?

**Practice.**

- Run units in sequence when they share files. Run them in parallel only when their file sets are disjoint and time matters.
- When a check fails, send it back to the subagent that made the change, if it is still within its limits. Otherwise start a fresh one with the item, the failing lines, and a summary of the diff.
