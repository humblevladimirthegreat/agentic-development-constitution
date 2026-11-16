# Article IV. Continuity

How work larger than one session gets from start to finish.

Follows from [premise 5](../charter.md#5-sessions-end): sessions end.

In practice: [A two-day documentation pass](../case-studies/a-two-day-documentation-pass.md).

## 4.1 State lives in the repository

**Statement.** Whatever the next session needs is in a file or in the version history. Nothing that matters lives only in a conversation.

**Why.** A conversation cannot be handed to the next session at a price worth paying, and often cannot be handed over at all. Work that depends on it has to be rediscovered, and the rediscovery is never quite the same as the original.

**Test.** Could a fresh session with no memory continue the work from a one-line prompt that names the plan?

**Practice.**

- The plan carries a status line: where the work stands, which parts are finished, and each decision the human has made, with its date.
- The version history holds what was done. The plan holds what is left.
- Before a session ends for any reason, the status line is brought up to date.

## 4.2 An idea and a plan are different documents

**Statement.** An idea says what the problem is, roughly what the fix would be, and what is undecided. A plan says how the work will be carried out. An idea becomes a plan only when the human wants it built and nothing blocks its first step.

**Why.** A plan written too early goes stale before anyone uses it. An idea that already contains a plan reads as a commitment. And an agent told to "implement the proposal" will start, whether or not the questions that block it were ever answered.

**Test.** Open any proposal in your repository. Can you tell in ten seconds whether an agent may start on it today?

**Practice.**

- Sort proposals into folders by readiness: ideas, ready, in progress, rejected. They move between folders, so refer to them by file name, not by link.
- An idea is short, under about 100 lines: problem, sketch, open questions. One or two worked examples say more than a specification.
- An idea is ready when no open decision blocks its first phase, it says exactly which part of the design it changes (or that it changes none), everything it depends on has landed, and its plan is complete.
- The agent writes the plan. The human settles the blocking decisions.
- When the work is done, the ordinary documentation owns the result, and the plan is deleted.

## 4.3 Cut the work where everything is green

**Statement.** A plan too long for one session is divided into phases. Each phase is one session's work, and each cut falls where everything before it is verified and committed.

**Why.** The budget in [2.4](02-context.md#24-end-the-session-before-the-context-is-full) forces a cut somewhere. A cut in the wrong place leaves the next session to inherit half-finished work with no record of what the first half assumed.

**Test.** At each cut in your plan, would the next session need the previous session's reports and diffs, or only its outcome?

**Practice.** Cut where:

- every check passes and every unit before the cut is committed,
- the next phase needs this phase's outcome, not its working material,
- a human decision is due, or later units depend on what earlier ones find,
- the next units read a different set of rules or files,
- a large automated rewrite and its hand fixes have just landed ([7.4](07-mechanization.md#74-fence-automated-rewrites)).

Never cut inside a unit.

At the end of a phase the agent runs the phase's gate, commits, rewrites the plan ([4.4](#44-rewrite-the-plan-at-every-cut)), and stops. It tells the human what the phase settled, which decisions are waiting, and the exact prompt that starts the next phase in a fresh session. It does not begin the next phase itself.

## 4.4 Rewrite the plan at every cut

**Statement.** At the end of each phase, and whenever the human says to stop partway, the agent rewrites the plan so that it holds only what the remaining work needs.

**Why.** A plan that only grows becomes a history, and the next session pays to read all of it on every call. What the next session needs is the remaining work and the few facts from finished work that it builds on.

**Test.** Does the plan get shorter as the work proceeds?

**Practice.**

- Delete finished items and their findings. The version history holds them.
- Drop background and rules that only finished work needed.
- Move each settled decision into the document that owns it, or onto the status line with its date.
- Write into the remaining items what they build on from finished work: new names, findings still to act on, rules that changed. This is input for later work, not a record of earlier work.
- Keep the remaining plan, the open decisions, and the point reached.
- Put the background last, so the next session reads from the top down and stops when it has enough.

When nothing planned is left, delete the plan. When the run ends for good with work left that nobody has planned, cut the document back down to an idea ([4.2](#42-an-idea-and-a-plan-are-different-documents)), with one clause naming what shipped and which document now owns it.
