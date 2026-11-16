# Article VII. Mechanization

What an agent should never do by hand, and how a rule becomes a tool.

Follows from [premise 3](../charter.md#3-a-rule-a-machine-can-check-should-be-checked-by-a-machine): a rule a machine can check should be checked by a machine. The same holds for a change a machine can make.

In practice: [Three days of agent spend](../case-studies/three-days-of-agent-spend.md), [A two-day documentation pass](../case-studies/a-two-day-documentation-pass.md).

## 7.1 Never by hand

**Statement.** A change that follows a rule goes through a tool that applies the rule everywhere, including every reference to the thing changed.

**Why.** An agent will happily make the same edit in forty places, one call at a time. Each call adds to the context, and each is a chance to miss a reference or to "fix" something nearby. A tool does all forty identically and reports the ones it could not do.

**Test.** Can the change be described as a rule? Then it should be a tool or a script, not a series of edits.

**Practice.**

- Build tools for the moves your project makes often: renaming something, moving a section, regenerating a derived table.
- A tool that moves or renames also rewrites every link and reference to what moved. Retargeting references by hand is the commonest source of silent breakage.
- List the never-by-hand operations in the instructions file, one line each. They qualify for that file because breaking them damages unrelated work without any error ([1.3](01-knowledge.md#13-procedures-load-on-demand)).
- What is left for the agent is what the tools cannot do: judgment, prose, placement.

## 7.2 Dry run by default

**Statement.** A tool that edits files prints what it would change unless it is told to write. It refuses to write a result it can tell is invalid.

**Why.** An agent calls tools with a confidence the tool cannot check. A dry run costs one extra call. A wrong bulk write costs a revert and a second attempt, or goes unnoticed.

**Test.** What happens when your bulk-edit tool is run with a wrong argument?

**Practice.**

- The default run prints the planned changes, including every reference it would rewrite. An explicit flag applies them.
- Before writing, the tool checks its own output against the checks it can run, and stops if they fail.
- Cases it cannot handle are listed for review, not guessed at.
- The tool says what it never touches, so the agent knows what remains to do by hand.

## 7.3 Scripts live in files and assert their anchors

**Statement.** A one-off script that edits files is written to a file and run from there, and it checks that each place it means to change matches exactly once before it writes anything.

**Why.** A script passed inline on a command line breaks on quoting, and each repair is another call. A script that finds its target by a loose pattern edits zero places or two, and reports success either way.

**Test.** If the text your script looks for were missing, would the script fail or finish quietly?

**Practice.** Choose between a direct edit and a script by the shape of the change:

- A direct edit suits short text: a line, a sentence, a table row. It needs the exact old text, so it fits text that is cheap to quote.
- A script suits whole sections cut or replaced, and the same change repeated in many places. It finds a section by its headings, so it never has to reproduce the old text.

One agent writes the script, runs it, and verifies the result once. Do not use one agent per row.

## 7.4 Fence automated rewrites

**Statement.** When a tool is about to make a large automated change, commit the hand-made work first, commit the tool's output on its own, and then continue by hand.

**Why.** A commit that mixes forty mechanical changes with three hand edits cannot be reviewed: the three are lost among the forty. Kept apart, the tool's commit can be skimmed as mechanical, reverted, or rerun without losing any hand work.

**Test.** Pick a large commit from your history. Can you tell which lines a tool wrote and which a person or agent chose?

**Practice.**

- Name the exact command in the tool commit's message.
- Hand fixes that the tool's review list calls for go in the next commit, not the tool's.
- This is one of the scoped standing permissions to commit ([3.4](03-authority.md#34-standing-permission-is-written-scoped-and-narrow)).

## 7.5 Prose, then tool, then hook

**Statement.** A rule starts as a sentence. If it keeps being broken, build a tool that makes the right action the easy one. If it is still broken, add a hook that blocks the wrong action and says what to do instead.

**Why.** Each step costs more to build than the one before, so none should be built before it is needed. But a rule that agents keep breaking will not be fixed by a firmer sentence, and each restatement adds weight to the instructions file.

**Test.** Which rule in your instructions file have you reworded more than once because agents kept breaking it?

**Practice.**

- A good blocking message names the alternative, with the exact command.
- Block narrowly. Stop the specific wasteful form and allow the neighboring forms that are fine.
- A hook fails open: if the hook itself errors, the action goes ahead. A broken guard should not stop the work.
- When a hook lands, shorten the prose rule to one line that points at it.
