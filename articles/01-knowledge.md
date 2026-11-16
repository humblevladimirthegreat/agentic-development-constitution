# Article I. Knowledge

Where the truth lives, and how an agent finds it.

Follows from [premise 2](../charter.md#2-each-fact-has-one-owner): each fact has one owner. It also leans on [premise 1](../charter.md#1-context-is-the-scarce-resource), because the cheapest knowledge is the knowledge an agent did not have to load.

In practice: [The instructions file that changed too often](../case-studies/the-instructions-file.md), [A twenty-item evaluation](../case-studies/a-twenty-item-evaluation.md).

## 1.1 One owner per fact

**Statement.** Every fact an agent may act on lives in exactly one file. Everything else links to that file and does not restate it.

**Why.** Copies drift. A person who meets two versions asks which is current. An agent acts on the one it loaded, and usually that is the shorter, older summary.

**Test.** Take a fact that changed in the last month. Search the repository for its old value. Every hit outside the owning file is a violation.

**Practice.**

- Keep a table that names, for each file, what it owns. The table is a pointer. The file is the authority.
- When you are about to write a sentence that says what another file says, write a link.
- Some copies are worth their cost, such as a rule restated in a subagent's definition so that the subagent never loads the long document. Then the owner lists every copy, so that one edit updates them all.

## 1.2 The instructions file routes; it does not teach

**Statement.** The file loaded into every session holds invariants, a routing table, and workflow rules. It never summarizes content that another file owns.

**Why.** A summary in the instructions file is a second copy that nothing keeps in sync. Agents trust the loaded file more than the document it summarizes, so a stale summary overrides a correct source. Every line of it is also paid for by every session, including the ones that never needed it.

**Test.** For any line in the file: would any of the last five substantive commits have forced you to edit it? If so, it belongs in the document those commits changed.

**Practice.** The file includes:

- Invariants that apply everywhere and rarely change.
- A routing table: each source file, with a line of topic nouns saying what it owns. It tells the agent which file to open, not what the file says.
- Workflow and tooling that most tasks need: how to build and test, what never to do by hand, how to handle large output.
- One pointer row per procedure ([1.3](#13-procedures-load-on-demand)).
- Judgment rules that belong to no other document, such as how to weigh options ([3.5](03-authority.md#35-weigh-options-by-their-effect-on-the-user-not-by-effort)).

It excludes anything a design change would force you to edit, anything already stated elsewhere, and second indexes of the routing table.

Keep it short enough to read in full. When a row grows past one line, move the detail into the file it points to.

## 1.3 Procedures load on demand

**Statement.** The step-by-step for one kind of task lives in its own file. The instructions file keeps one row for it: the task in a few words, and the file to read in full before starting.

**Why.** Steps that only one kind of task needs should be paid for only by sessions doing that task. A subagent briefed for one job should not carry the steps for nine others.

**Test.** Does this step matter only to one family of tasks? Then it belongs in that family's procedure. There is one exception: if breaking the step causes silent damage in unrelated work, state it in one line in the instructions file and link the procedure.

**Practice.**

- A procedure holds commands, flags, order, and what to check afterward.
- Write procedures as plain files, not in one tool's own format. Any agent can follow a path, and a single copy cannot drift from a second.
- Policy and rationale stay in their own documents. A procedure links to them.
- A custom subagent's definition names the procedures its task needs.

## 1.4 Generate the prompt from the source

**Statement.** When an agent needs domain knowledge for one specific input, a script assembles exactly the rules that input involves and the nearest worked examples, drawn from the owning documents.

**Why.** "Read the documentation" loads far more than the task needs and leaves the selection to the agent. A prompt written by hand is a second copy of the rules. A generated one is neither.

**Test.** Can the agent do the task from the generated prompt alone? When the prompt and a document disagree, is it clear that the document wins and the generator gets fixed?

**Practice.**

- Analyze the input with ordinary code first (parse it, classify it), and use the result to select rules and examples.
- Keep the prompt self-contained, so that it works for a small model with no other context.
- Score the generator with examples held out of its own pool ([6.7](06-verification.md#67-free-checks-first-paid-evaluations-on-request)).

## 1.5 Record the no

**Statement.** A rejected idea, or a feature that is absent on purpose, is written down with its reason.

**Why.** An agent that sees a gap offers to fill it. Without a record, every new session proposes the same natural idea again, and sooner or later one of them builds it. The reason matters as much as the verdict, because it lets a later reader judge whether the rejection still holds.

**Test.** Can a fresh session tell "nobody has decided this" from "this was decided against"?

**Practice.**

- Keep one document for decisions against: what was rejected, the alternatives, and why.
- Keep a list of known gaps, each marked open (not decided yet) or closed (decided against).
- Keep rejected plans as they were, with the date and the reason on the first line.
- Do not restate what was decided *for*. The ordinary documentation owns that.
