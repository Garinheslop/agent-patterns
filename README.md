# agent-patterns

Patterns for running 12+ parallel Claude Code sessions on the same repo without the work colliding.

These are extracted from the operating doctrine I use at [GYNERGY](https://gynergy.com), where I run a small team of humans alongside a much larger team of agents. The patterns are language-agnostic, framework-agnostic, and mostly about coordination — which is the hard part. Writing code is the easy part.

## The patterns

| # | Pattern | One-line |
|---|---------|----------|
| 1 | [Planner DAG before parallelism](./patterns/01-planner-dag.md) | No DAG, no parallelism. |
| 2 | [One file, one owner](./patterns/02-one-file-one-owner.md) | Two agents on the same file is a merge conflict you scheduled in advance. |
| 3 | [Cold-context reviewer](./patterns/03-cold-context-reviewer.md) | The agent that built it cannot review it. |
| 4 | [Five-vector reconnaissance](./patterns/04-five-vector-recon.md) | Before claiming "X isn't built," run all five vectors. |
| 5 | [Critical-file safeguards](./patterns/05-critical-file-safeguards.md) | Some files are load-bearing enough that every edit needs a backup, a deletion count, and a second look. |
| 6 | [Autonomy gradient](./patterns/06-autonomy-gradient.md) | Match the trust level to the blast radius. |

## Why these matter

I ran a single Claude Code session for the first few months. It was great. Then two sessions on the same repo. Tolerable. Then four. Painful. Then twelve.

At twelve, every problem stops being about the model and starts being about the operating system around the model. These patterns are what made twelve work.

If you're a solo builder or a small team using AI coding agents heavily, you'll probably hit the same wall. Steal whatever's useful.

## Companion repos

- [claude-code-starter](https://github.com/Garinheslop/claude-code-starter) — a minimal `.claude/` skeleton that implements many of these patterns as hooks and slash commands.
- [incident-postmortems](https://github.com/Garinheslop/incident-postmortems) — the production incidents that taught me why each of these patterns is non-negotiable.

## License

MIT. Use it, modify it, make it yours. Use at your own risk.
