# Article VI. Verification

How anyone, human or agent, knows the work is right.

Follows from [premise 3](../charter.md#3-a-rule-a-machine-can-check-should-be-checked-by-a-machine): a rule a machine can check should be checked by a machine. It also serves [premise 1](../charter.md#1-context-is-the-scarce-resource), because how a check reports matters as much as what it checks.

In practice: [Three days of agent spend](../case-studies/three-days-of-agent-spend.md), [A two-day documentation pass](../case-studies/a-two-day-documentation-pass.md), [A twenty-item evaluation](../case-studies/a-twenty-item-evaluation.md).

## 6.1 One quiet check

**Statement.** One command runs every check and prints a verdict, the errors that are new, and nothing else.

**Why.** Raw build and test output is the largest thing an agent routinely reads, and almost none of it is about the change just made. In a project with known failures, the agent also cannot tell its own errors from the ones that were already there.

**Test.** After a one-line change, how many lines does your check print when it passes? When it fails?

**Practice.**

- Save a baseline of the current errors at the start of the work.
- The check prints pass or fail, the errors new since the baseline, and a count of those resolved, in about 30 lines at most.
- The full output goes to a log file, to be searched when needed.
- Offer a fast mode for use during an item and the full run for its end.
- Run it once per unit. Never repeat a passing run when nothing has changed since.

## 6.2 A failure names its fix

**Statement.** Every failure ends with a short footer: the file and line, what was expected or how to fix it, and a link to the rule that was broken.

**Why.** An agent given a bare failure reruns the check, opens the full log, or reads the checker's source code to work out what it wants. Each of those is context spent on a question the checker could have answered.

**Test.** Can the agent fix a failure from the footer alone?

**Practice.**

- Write each check's message for a reader who has never seen the rule.
- Link the message to the section that owns the rule ([1.1](01-knowledge.md#11-one-owner-per-fact)), so the agent reads one section and not the whole policy.
- State it in the instructions file: read the footer, and never rerun a failed check before editing.

## 6.3 Check what the documentation shows

**Statement.** Whatever in the documentation a machine can validate is validated on every build: code samples, examples, cross-references, the terms used.

**Why.** Agents learn a project from its documents. A wrong example gets copied into new work, and a broken reference sends the agent searching. Documentation that is only reviewed by people drifts from the implementation within weeks.

**Test.** If the implementation changed tomorrow so that an example in your documentation became invalid, what would tell you?

**Practice.**

- Run every example through the real implementation as part of the build.
- Check that links and anchors resolve, including the ones in error messages.
- Where documents have a reading order, check that nothing relies on a later page.

## 6.4 Guard the scope of a change

**Statement.** When a task is limited to one kind of change, a check proves that the diff stayed inside the limit.

**Why.** "Improve the wording, change nothing else" is an instruction that an agent mostly follows. A reviewer facing forty files cannot find the one place where it did not.

**Test.** For your last constrained task, how did you confirm that the constraint held?

**Practice.**

- For a wording-only pass, fail the check when headings, anchors, code, links, or examples differ from the starting revision.
- For a move, compare the content before and after and fail on any difference.
- For review, list the sections that changed, plus the one after each, and scope the review to that list ([5.6](05-delegation.md#56-review-with-eyes-that-did-not-write)).

## 6.5 Compare behavior, not only code

**Statement.** When the implementation changes, run the old version and the new one over the same inputs and classify every difference.

**Why.** Tests show that what you thought to test still works. A comparison over a large body of real inputs shows everything that changed, including what nobody thought to test.

**Test.** After your last change to core logic, could you list every input whose result changed?

**Practice.**

- Use the examples in the documentation as the body of inputs. They already exist and already matter.
- Extend it with small variations of each input, to reach cases that no example shows.
- Class each difference: a result changed, an input is newly accepted, an input is newly rejected, or only a message changed.

## 6.6 Ratchet what you cannot fix at once

**Statement.** A rule that existing work already breaks in many places starts as a count per file. The count may fall and may never rise.

**Why.** A new rule that fails the build on day one gets switched off. A rule that only warns gets ignored. A ratchet stops new violations at once and lets the old ones be cleared at leisure.

**Test.** Which of your warnings has anyone acted on this month?

**Practice.**

- Exact rules fail the build. Heuristic rules print one count each and fail only when a file's count rises above its stored baseline.
- Update the baseline deliberately, as its own step, when violations are cleared.

## 6.7 Free checks first, paid evaluations on request

**Statement.** Deterministic metrics are the regression net and run freely. An evaluation that calls a model costs money, so it runs only when asked for, at the smallest size that answers the question.

**Why.** Model-judged evaluations are the only way to measure some things, such as whether a generated prompt really lets a small model do the task. They are also slow, noisy, and billed per run, and an agent left to itself will run a full batch for every variant.

**Test.** What did your last evaluation run cost, and what decision did it change?

**Practice.**

- Build free metrics that predict the paid result, give them a stored baseline and a tolerance, and fail when a headline number falls.
- In the paid evaluation, a small model does the task with the reference answer held out, and a stronger model judges the result.
- Decide for free wherever you can: an exact match with an accepted answer is right, and an output that does not even parse is wrong, with no further call.
- Store each result keyed by its prompt, so that a rerun pays only for what changed.
- Start with a small balanced subset, reuse stored results as the baseline, extend only when the result is ambiguous, and report the cost.
