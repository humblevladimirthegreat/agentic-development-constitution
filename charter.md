# Charter

Six premises. Every principle in the articles follows from at least one of them, and each article says which.

A premise is a claim about how agents and projects behave, not a rule. If one of them stops being true, because models change or pricing changes, the principles built on it are due for review ([8.3](articles/08-amendment.md#83-a-rule-enters-with-a-failure-and-leaves-with-a-reason)).

## 1. Context is the scarce resource

An agent does not remember a conversation. On every call it is sent the whole of it again: the instructions, every file it has read, every command output, every message. So a thing read once is paid for on every later call, and a session's cost grows much faster than its length. Attention suffers too. Detail that matters gets harder to find among thousands of lines that no longer do, and when the context fills, something is summarized away.

Most of the waste in agent work is context spent on things the agent did not need to see.

*Derived:* [Article II, Context](articles/02-context.md), and much of [Article V, Delegation](articles/05-delegation.md).

## 2. Each fact has one owner

An agent trusts what it has loaded. It cannot tell a current statement from a stale copy of it, and when two files disagree it tends to follow whichever it read first, which is usually the summary and not the source. Every second copy of a fact is therefore an error waiting for the first edit that updates only one of them.

So each fact lives in one file, and everything else points to that file.

*Derived:* [Article I, Knowledge](articles/01-knowledge.md).

## 3. A rule a machine can check should be checked by a machine

A rule written in prose depends on the agent having read it, remembering it late in a long session, and judging that it applies. Each of those fails some of the time. A check that runs on every change fails none of the time, costs the agent nothing to remember, and tells it exactly what to fix.

Prose is where a rule starts. It is the right home only for rules that need judgment.

*Derived:* [Article VI, Verification](articles/06-verification.md), and [Article VII, Mechanization](articles/07-mechanization.md).

## 4. The human owns decisions; the agent owns execution

An agent that meets a gap fills it with something plausible. For execution that is what you want. For a decision about what the product should be, it means the choice was made by nobody, inside a diff, and later work is built on it before anyone notices.

The opposite failure is as real: an agent that asks about everything gets nothing done. So the line between the two kinds of choice is written down, and the agent has a way to keep working when it meets one that is not its own.

*Derived:* [Article III, Authority](articles/03-authority.md).

## 5. Sessions end

Every session ends: it runs out of context, the work pauses overnight, or it is cut on purpose to keep cost down. Whatever lived only in the conversation is then gone. The next session starts knowing nothing but what the repository tells it.

So anything the work depends on is written into the repository before the session ends, in a form short enough that the next session can afford to read it.

*Derived:* [Article IV, Continuity](articles/04-continuity.md).

## 6. Rules are measured, then changed

Rules for agents pile up. Each one was added for a reason, each costs context in every session, and nobody removes them. Models also change, so a caution that was necessary last year may now only cost tokens.

A rule earns its place by preventing a failure that was observed, and it keeps its place only while that is still true. That needs measurement, and a routine way to notice waste.

*Derived:* [Article VIII, Amendment](articles/08-amendment.md).
