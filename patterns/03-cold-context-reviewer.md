# 03. Cold-context reviewer

**Principle.** The agent that wrote the code cannot review it. Before any PR is marked ready, a second agent — with no knowledge of the prior conversation — reads the diff and the source and produces an independent critique.

## Why

The implementing agent has spent its entire context window rationalizing the approach. Every shortcut, every "we can fix that later," every silent failure path has been internalized as "the agreed-upon design." Asking that same agent to review its own work produces a confirmation, not a review.

The cold reviewer hasn't agreed to anything. It sees:

- Scope creep ("the task was X, but Y was also changed")
- Silent failures (try/catch blocks that swallow errors with no logging)
- Hallucinated completions ("added tests" — but there are no test files in the diff)
- Pattern drift (this PR uses a different convention than the rest of the codebase)

These are the exact things the implementing agent talked itself out of caring about.

## Mechanism

The reviewer agent starts in a fresh context with three inputs and nothing else:

1. The task description (what was supposed to happen)
2. The diff
3. The source files touched

Crucially, **not** the conversation that produced the change. That's the bias you're trying to eliminate.

It produces a critique against a small fixed checklist:

- Does the diff do what the task claimed?
- Is anything in the diff outside the stated scope?
- Are there silent failure paths?
- Are there missing tests where tests existed before?
- Does the code match the conventions of the surrounding files?

Output is a verdict — ship / hold / block — with line-level evidence.

## Anti-pattern

Asking the implementing agent "are you sure?" or "review your own work." This produces a longer rationalization, not a review. It also burns context, which is worse than burning a fresh agent.

## Heuristic

If your reviewer agent shares any conversation history with the implementer, it's not a reviewer. It's a second opinion from the same person, and you already know what they think.

## Related

- [04. Five-vector reconnaissance](./04-five-vector-recon.md) — the same principle (don't trust the first pass) applied to discovery.
