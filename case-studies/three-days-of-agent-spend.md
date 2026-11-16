# Three days of agent spend

What a project learned by measuring where its agents' tokens went, and what it built in response.

**Principles shown:** [2.1](../articles/02-context.md#21-know-where-the-tokens-go), [2.2](../articles/02-context.md#22-read-slices-not-files), [2.3](../articles/02-context.md#23-delegate-a-read-when-only-its-conclusion-is-needed), [2.5](../articles/02-context.md#25-load-only-what-the-task-uses), [5.1](../articles/05-delegation.md#51-delegate-to-buy-a-fresh-context), [5.3](../articles/05-delegation.md#53-subagents-are-short-lived-and-stop-themselves), [5.5](../articles/05-delegation.md#55-fit-the-agent-to-the-unit), [6.1](../articles/06-verification.md#61-one-quiet-check), [6.2](../articles/06-verification.md#62-a-failure-names-its-fix), [7.5](../articles/07-mechanization.md#75-prose-then-tool-then-hook), [8.2](../articles/08-amendment.md#82-measure-from-the-transcripts).

## The project

A documentation-heavy repository with a custom parser and about 1,450 commits, worked on daily with a coding agent. Large changes ran as plans carried out by an orchestrating session and its subagents.

## The measurement

The agent's own transcripts record every call and its token counts. A script read three days of them: 28 sessions, 21 subagents, 3,517 calls. It weighted each kind of token by its relative price at the time (fresh input 1, cached input 0.1, writing to the cache 1.25 to 2, output 5), so the totals describe cost, not raw counts.

What it found:

- **Most of the spend was re-reading.** Cached context sent again on later calls was 66% of the total. Context beyond the first 80k tokens of each call was 32%.
- **Sessions ran long and large.** A main session started at about 46k tokens, before any work, and reached a median peak of about 205k over a median of 74 calls.
- **Big reads dominated exploration.** Results larger than 2k tokens made up 55 to 59% of what exploring cost. Agents opened large saved outputs whole 10 times, about 156k tokens. They read about 20k tokens twice.
- **Failed checks set off loops.** Of 333 check runs, 91 failed. After a failure, agents reran the check 47 times without editing anything, opened the full log 15 times, and searched the checker's source code 20 times to work out what it wanted.
- **Unused capabilities were not free.** The list of available skills took about 19.5k characters of every context and about 1.8% of spend. No skill was invoked in the whole window.
- **A subagent started at about 38k tokens** before its first edit.

One piece of arithmetic came out of this. Text of T tokens read into a context with about 70 calls still to run costs about 9 × T over the rest of the session: the write, plus a tenth of T on each later call. The short subagents in the window cost between 21k and 120k weighted tokens. So a read of more than about 15k tokens, when only its conclusion was needed, was cheaper to hand to a subagent.

## What changed

This is what the project now does about each finding.

| Finding | What answers it |
|---|---|
| Re-read context dominates | Plans were cut into phases of about six units, each run in a fresh session that reads only the rewritten plan. |
| Long subagents | One or two items per subagent, a brief in a file, and a standing line telling the subagent to check its progress at about 25 calls. |
| Raw check output, and reruns | One quiet check that compares against a saved baseline and prints only new errors, in about 30 lines. |
| Log reads and source searches after a failure | A footer on every failure with the file and line, the fix, and a link to the rule. |
| Every tool on every subagent | A worker agent with four tools. |
| Skills listed and never used | Most skills set to load only when asked for. |
| The 15k break-even | A rule in the instructions file naming that threshold for delegating a search. |

The quiet check arrived during the window, and the spend report and the narrow worker at its end. After the delegation rules were rewritten, subagents reached a median peak of about 64k tokens at a median cost of about 137k weighted tokens. No clean figure from before the rewrite exists to set beside those, which is itself a lesson: the baseline should have been saved first.

## One rule, three forms

The ten whole reads of saved outputs got the same treatment in three steps.

The first step was a sentence. Right after the measurement, the instructions file gained a rule: never read a saved output whole, and before reading a long document, list its headings and read only the ranges you need.

A sentence depends on the agent recalling it deep into a long session, so the project did not stop there. About a week later it added an outline command that prints a document's headings with line ranges and approximate token counts. Reading a slice became the easy action.

With the command came a hook. It refuses a command that would print a long document whole, and it replies with the commands for reading the document in slices. It blocks narrowly: a search through the same file, or a read of a line range, passes. If the hook itself fails, the command runs.

## What to take from it

- Carried context was most of the bill. Nothing in the daily experience of using the agent had made that visible.
- The transcripts already held everything the report needed. Nothing had to be instrumented.
- Keep the baseline where the report prints it. The next change to the rules is then read against a number, not a memory.
