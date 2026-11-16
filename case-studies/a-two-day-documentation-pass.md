# A two-day documentation pass

How one large, repetitive change was carried through nine sessions without any session needing to remember the one before.

**Principles shown:** [3.2](../articles/03-authority.md#32-log-it-recommend-skip-carry-on), [4.3](../articles/04-continuity.md#43-cut-the-work-where-everything-is-green), [4.4](../articles/04-continuity.md#44-rewrite-the-plan-at-every-cut), [5.6](../articles/05-delegation.md#56-review-with-eyes-that-did-not-write), [6.4](../articles/06-verification.md#64-guard-the-scope-of-a-change), [6.6](../articles/06-verification.md#66-ratchet-what-you-cannot-fix-at-once), [7.4](../articles/07-mechanization.md#74-fence-automated-rewrites).

## The project

A repository with about 170 pages of user-facing documentation, kept in step with a custom parser. The job was a style pass: bring the wording of every page in line with the project's style guide, without changing what any page taught.

That is an awkward job for an agent. It is far too large for one session. Every unit looks like every other, so mistakes repeat. And the instruction "change the wording only" is one an agent follows almost always, which is not the same as always.

## Tools before the first page

The plan's first three units were tooling. They built what the rest of the run would lean on:

1. **A ratchet.** The style rules that could be checked by pattern became warnings with a stored count per page. A page's count could fall and could not rise. That allowed a rule to be switched on across 170 pages that already broke it.
2. **A scope guard.** One check listed the sections that had changed since a given revision, plus the section after each. Another failed if an edit had touched anything but wording: a heading, an anchor, a code sample, a link, an example.
3. **A reviewer.** A subagent definition for a style review, to be run in a fresh context that had made none of the changes.

Only then did the editing start.

## The run

The remaining 38 units were editing, with the pages grouped by section of the documentation. By the project's procedure, each unit goes to a fresh subagent with a brief and ends in one commit after its check passes.

The work was cut into nine phases. At the end of each of the first eight, the orchestrating session rewrote the plan: finished units deleted, their findings folded into the units still to come, the open decisions kept. It committed that rewritten plan and stopped. By the project's rule, the next phase began in a new session that read only that plan.

Decisions that were the human's to make did not stop the run. Those that came up in the first phase were logged and the work carried on. The human answered them in a batch, and one commit applied the answers before the second phase began.

One change in the middle was mechanical: a section belonged on a different page. It went through the project's move tool, which cuts the section, places it, and rewrites every link to it. That commit holds nothing but the tool's output, and its message is the exact command that produced it. The hand edits the move made necessary are in the commit after.

The ninth phase closed the run and deleted the plan. The documentation owns the result, and the commit history holds the story.

## The result

Two days, 57 commits, 159 files changed, about 3,100 lines added and 2,800 removed.

Trusting it did not depend on reading 159 files. The scope guard shows whether anything but wording moved. The ratchet fails the build if a warning count rises. The review ran in a context that had made none of the edits. And the one structural change sits alone in a commit that can be rerun.

## What to take from it

- Build the checks first. Three units of tooling made thirty-eight units of editing reviewable.
- The plan got shorter with every phase, which is how you know it was being used as a plan and not kept as a diary.
- A plan that is deleted at the end leaves nothing to go stale.
