# A twenty-item evaluation

How a small paid evaluation, costing under five dollars, decided what to build next.

**Principles shown:** [1.4](../articles/01-knowledge.md#14-generate-the-prompt-from-the-source), [3.4](../articles/03-authority.md#34-standing-permission-is-written-scoped-and-narrow), [6.7](../articles/06-verification.md#67-free-checks-first-paid-evaluations-on-request).

## The project

The project has its own formal notation, defined across many pages of documentation. One of its tools takes an English sentence and generates a prompt: the rules that sentence is likely to need, the relevant vocabulary, and the nearest worked examples, all drawn from the documentation. A small model with no other context should be able to write the sentence in the notation from that prompt alone.

## The free metric, and the doubt about it

Improving the tool went in rounds, and each round needed a score. The project had a free one. For each test sentence with a known correct answer, it checked whether the generated prompt contained every piece that answer used. If it did, the prompt was called *self-contained* for that sentence.

That number costs nothing and runs in seconds. It serves as the regression net, with a stored baseline and a tolerance. The doubt was whether it measured the right thing. A prompt can contain every piece and still not produce a correct answer.

The paid evaluation exists to answer that. A small model writes the answer from the prompt, with the reference held out. An exact match is right, and an answer that does not parse is wrong, both at no further cost. Anything else is translated back and judged by a stronger model. Results are stored by prompt, so a rerun pays only for what changed. The project's rule is that this runs only when asked for.

## The request

Midway through the work the question came up: of the misses, how many were the prompt's fault for lacking something, and how many were the model's fault for misusing what it had? The answer would decide whether the next round went into coverage or into something else.

The instruction to the agent was to run the least that would settle the question, not a full batch for each variant.

So it did three things:

- It took 20 items from an earlier run, half of them self-contained and half not, and reused that run's stored answers as the baseline. The baseline cost nothing.
- It ran two variants on those same 20. One added the correct answer's pieces to the prompt, in alphabetical order, so that any miss could only be a mistake of arrangement. The other added the correct answer's outline, in order, so that any miss could only be a piece the model could not spell from the prompt.
- It reported the cost: $4.38 for both runs.

## What it showed

| The same 20 items | Plain prompt | Pieces given | Outline given |
|---|---|---|---|
| Misses, all | 55% | 30% | 50% |
| Misses, self-contained (10) | 30% | 30% | 30% |
| Misses, not self-contained (10) | 80% | 30% | 70% |

Three things followed, and each named a different piece of work.

- **The free metric was sound, for half the problem.** Given the missing pieces, the hard items fell from 80% misses to 30%, exactly the rate of the items that had been self-contained all along. Raising the free number was worth doing.
- **What was missing was not what anyone had assumed.** Even with the whole outline given, the model left out or misspelled the same handful of pieces. None was ordinary vocabulary. All were fixed keywords of the notation that the prompt had linked to and never spelled out. The fix was to show each small table of keywords whole.
- **The remaining 30% was arrangement.** With every piece supplied, most of the misses put them in the wrong order. One sentence in the prompt said that certain words could go anywhere. For these cases that was wrong.

With 20 items the percentages are not to be trusted to the digit, and the write-up said so. The direction, and the individual failures read one by one, were enough to set the next three tasks in order.

## What to take from it

- A small run aimed at one question is worth more than a large run aimed at a score.
- Design the variants so that each one removes a single cause. The gap that remains is then the other cause.
- Store every result. The baseline for this run had been paid for nine days earlier.
- Paid runs are one of the actions an agent takes only on request, and it reports what it spent.
