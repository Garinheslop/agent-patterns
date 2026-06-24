# agent-patterns

**How a small team ships like a large one.**

GYNERGY runs on a fleet of AI coding agents. Over the last 21 months that fleet has written close to 10,000 commits across the platform: a coaching company, an AI coach, and the internal agent stack that runs the business itself. The large majority of that code was written by agents I orchestrate, not typed by hand. That is the entire point.

This repo is the operating system that lets a dozen agents work in parallel on the same codebase without the work colliding. It is language-agnostic and framework-agnostic, because the hard part was never writing the code. The hard part is coordination.

## Why this exists

I ran a single Claude Code session for the first few months. It was great. Then two sessions on the same repo. Tolerable. Then four. Painful. Then twelve.

At twelve, every problem stops being about the model and starts being about the system around the model. Two agents grab the same file. An agent reviews its own work and misses the bug it just wrote. A session declares a feature missing and rebuilds something that already shipped. None of that is a model failure. It is an operating-system failure.

These patterns are that operating system. They are what made twelve work.

## The patterns

| # | Pattern | The rule |
|---|---------|----------|
| 1 | [Planner DAG before parallelism](./patterns/01-planner-dag.md) | No dependency graph, no parallelism. Fan-out without a plan is collisions scheduled in advance. |
| 2 | [One file, one owner](./patterns/02-one-file-one-owner.md) | Two agents on the same file is a merge conflict you booked ahead of time. |
| 3 | [Cold-context reviewer](./patterns/03-cold-context-reviewer.md) | The agent that wrote it cannot review it. Bring in one that never saw the conversation. |
| 4 | [Five-vector reconnaissance](./patterns/04-five-vector-recon.md) | Before claiming "X is not built," run all five searches. Most "missing" features already exist. |
| 5 | [Critical-file safeguards](./patterns/05-critical-file-safeguards.md) | Some files are load-bearing enough that every edit needs a backup, a deletion count, and a second look. |
| 6 | [Autonomy gradient](./patterns/06-autonomy-gradient.md) | Match the trust level to the blast radius. Reads run free. Migrations stop and ask. |

## Why each one is here

Every pattern in this repo exists because something broke in production first. The fix was never "be more careful." The fix was a hook, a CI check, or a typed contract that makes the same mistake impossible to repeat. A postmortem that ends in "the team will be more careful" ships the same bug next quarter. A postmortem that ends in a machine-enforced rule turns a bad day into permanent uplift.

The incidents that taught me each rule are public too, in [incident-postmortems](https://github.com/Garinheslop/incident-postmortems).

## Who this is for

Solo builders and small teams running AI coding agents heavily enough to hit the wall. If you are running one agent, you do not need this yet. If you are running ten, you needed it yesterday. Steal whatever is useful.

## Companion repos

- [claude-code-starter](https://github.com/Garinheslop/claude-code-starter) · a minimal `.claude/` skeleton that implements many of these patterns as hooks and slash commands. The how, in code.
- [incident-postmortems](https://github.com/Garinheslop/incident-postmortems) · the production incidents that made each pattern non-negotiable.

## License

MIT. Use it, fork it, make it yours. Use at your own risk.
