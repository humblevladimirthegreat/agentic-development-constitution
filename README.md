# Agentic Development Constitution

A set of working principles for building software with AI coding agents. It says how a project should be arranged so that an agent can do long, real work in it: what the agent is told, what it may decide, how its work is checked, and how the rules themselves change.

It is a living document. Principles are added when something goes wrong or a measurement shows waste, and they are removed when they stop earning their cost. The commit history shows what changed and when.

## Who it is for

- Developers who already use a coding agent and want it to hold up on work that spans many sessions.
- Teams deciding how much to hand to agents and what must stay with people.
- Anyone evaluating how this author works.

It assumes you have used a coding agent. It does not assume any particular vendor, model, or tool, and the principles name none.

## How to read it

Start with the [charter](charter.md). It is one page and holds the six premises that everything else follows from. If you disagree with a premise, the articles that derive from it will not persuade you, and you will know which ones to skip.

Then read the articles in order, or go straight to the one that matches your problem.

| Article | Read it when |
|---|---|
| [I. Knowledge](articles/01-knowledge.md) | The agent acts on stale or contradictory information, or your instructions file keeps growing. |
| [II. Context](articles/02-context.md) | Sessions are expensive, slow, or lose track of details late in a long run. |
| [III. Authority](articles/03-authority.md) | The agent makes decisions you wanted to make, or stops to ask about ones you did not. |
| [IV. Continuity](articles/04-continuity.md) | Work is too large for one session and each new session starts by rediscovering the last one. |
| [V. Delegation](articles/05-delegation.md) | You run subagents and they cost more, or report worse, than you expected. |
| [VI. Verification](articles/06-verification.md) | The agent says the work is done and you cannot tell whether it is. |
| [VII. Mechanization](articles/07-mechanization.md) | The agent makes the same edit in forty places by hand, or keeps breaking a rule you wrote down. |
| [VIII. Amendment](articles/08-amendment.md) | You want the rules to improve with use and not only accumulate. |

## Case studies

The articles state principles and give no evidence for them. The evidence is in the case studies: accounts of what happened on a real project, with the numbers, and with links back to the principles each one shows.

| Case study | What it shows |
|---|---|
| [Three days of agent spend](case-studies/three-days-of-agent-spend.md) | A project measures where its tokens go, finds that two thirds of the bill is re-read context, and builds tools in response. |
| [A two-day documentation pass](case-studies/a-two-day-documentation-pass.md) | A 159-file change carried through nine sessions, with the checks built before the first edit. |
| [The instructions file that changed too often](case-studies/the-instructions-file.md) | A file edited in 44% of commits becomes one edited in 16%, by ceasing to summarize. |
| [A twenty-item evaluation](case-studies/a-twenty-item-evaluation.md) | A paid evaluation costing under five dollars decides what to build next. |

All four come from the author's own work. Read each figure as an observation from one setup, and measure your own ([8.2](articles/08-amendment.md#82-measure-from-the-transcripts)).

## Layout

```
README.md          this page
charter.md         the six premises
articles/          one file per article, I to VIII
case-studies/      what happened when the principles were applied
LICENSE            CC BY 4.0
```

## How a principle is written

Every principle has a number (`2.3` is the third principle of Article II) and four fields.

| Field | Holds |
|---|---|
| **Statement** | The principle in one or two sentences. |
| **Why** | The specific failure it prevents. |
| **Test** | A question you can ask of your own project to tell whether you are breaking it. |
| **Practice** | How to do it, with no tool or vendor named. |

The Test is the field that matters most. A principle without one is an opinion, and an agent cannot check itself against an opinion.

## Terms

| Term | Meaning here |
|---|---|
| Agent | An AI model that reads files, runs commands, and edits a repository through tools. |
| Session | One conversation with an agent, from its first message until it ends or is reset. |
| Context | Everything the model is given on one call: instructions, the conversation so far, every file and command output it has read. |
| Turn, call | One request to the model. The whole context is sent again on each one. |
| Instructions file | The file an agent loads at the start of every session in a repository. |
| Procedure | The step-by-step for one kind of task, kept in its own file and read only when that task comes up. |
| Subagent | An agent started by another agent to do one piece of work in a separate context. |
| Orchestrator | The session the human talks to when subagents are in use. It plans, briefs, checks, and commits. |
| Plan | A document describing a change too large for one edit: its units, checks, and commits. |
| Unit | One piece of a plan, small enough for one subagent or one sitting. |
| Gate | A check that must pass before the work moves on. |
| Green | The state in which every check passes. |

## Not here yet

An adoption kit is planned and not yet written: which principles to take up first on a small project, a per-project file that maps each article to that project's own commands and files, and a starter instructions file.

## License

Text licensed under [CC BY 4.0](LICENSE) ([summary](https://creativecommons.org/licenses/by/4.0/)). Reuse and adapt it freely, with credit to Jordan Myers and a link to this repository.
