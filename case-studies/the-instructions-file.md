# The instructions file that changed too often

What happened when a project stopped using its instructions file to explain the project.

**Principles shown:** [1.1](../articles/01-knowledge.md#11-one-owner-per-fact), [1.2](../articles/01-knowledge.md#12-the-instructions-file-routes-it-does-not-teach), [1.3](../articles/01-knowledge.md#13-procedures-load-on-demand).

## The project

A repository whose design lives in its documentation: dozens of source files, each owning one part of the design, with a parser and a set of tools kept in step with them. The design was still changing week to week.

## The first version

The instructions file already had the right skeleton: a table listing each source file. But each row tried to be helpful. Beside the file's name it gave a summary of what the file said: the main rules, the specific forms the file defined, and what each one meant. An agent could start work without opening anything.

The file was only 78 lines, but the lines were long. It ran to about 35,000 characters, all of it loaded into every session.

It was also a second copy of the design. Whenever the design changed, a row had to change with it. In the project's first 694 commits, the instructions file was edited in 305 of them, which is 44%.

The edits that were made are not the real cost. The real cost is the edits that were missed. Nothing checked the summaries against the documents they summarized. The build validated every example in the documentation and none in the instructions file. And an agent trusts the file it was handed at the start over a document it opened later, so a stale row could overrule a correct page.

## The policy

The project wrote down what the file is for, and gave each line one test: *would any of the last five design commits have forced an edit to this line?* If so, the line belonged in the document those commits had changed.

What remained:

- A few invariants that hold everywhere and almost never change.
- A routing table: every source file, with a line of topic nouns saying what it owns. It tells an agent which file to open. It does not say what the file says.
- Tooling and workflow rules that most tasks need.
- One row per procedure, naming the task and the file to read before starting it.

The step-by-step instructions for particular tasks later moved out into procedure files. There are eight, and together they run to 1,430 lines. A session loads one only when it takes on that task.

## The result

The file now has more lines, 129, and many more rows, because the table now covers many more files. It is also about 40% smaller, at roughly 20,000 characters, because a row is now a few nouns.

What changed most is how often it has to change. In the 757 commits since the policy, the file was edited in 119, which is 16%.

The comparison is rough. The work in the two periods was not the same, and other habits changed alongside. But the direction matches the reasoning: a file that names owners changes when ownership does, and a file that summarizes changes whenever anything does.

## What to take from it

- Line count is the wrong measure of an instructions file. Ask how often it must change, and whether anything would notice if it were wrong.
- The five-commit test is quick to run on your own file, line by line.
- A summary feels like a kindness to the agent. It is a copy, and the agent will believe it.
