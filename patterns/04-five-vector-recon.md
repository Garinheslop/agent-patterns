# 04. Five-vector reconnaissance

**Principle.** Before claiming "feature X isn't built" or "we should build Y," run all five discovery vectors. Reporting a missing feature without completing the recon produces false negatives that cost hours.

## Why

A single grep is a single hypothesis. The thing you're looking for may exist:

- Under a different name than you expected
- In a directory you didn't think to check
- Wrapped in a parent component that's wired into a layout, not a page
- Dormant — built but not currently rendered
- In an `_archive` folder where a refactor parked it

Each of those is invisible to a naive search, and each one has burned a real session of work.

The five vectors are designed so that anything built within the last two years on this codebase shows up in at least one of them.

## The five vectors

### Vector 1 — Direct name grep
The lazy default. Search for the exact name you'd expect:

```bash
grep -rln "OnboardingTour" .
```

### Vector 2 — Generic capability grep
Search for what the feature *would do*, not what it'd be named. For "onboarding tour" try: `tour`, `walkthrough`, `guide`, `wizard`, `tutorial`, `intro`, `orientation`, `coach`, `companion`, `presence`, `floating`. The system you want may have been rebuilt under a different name.

### Vector 3 — Directory listing
Stop greppping and just look:

```bash
find components -type d
ls components/onboarding/ 2>/dev/null
```

A 30-second `find` beats a 30-minute grep chain.

### Vector 4 — Archive and dormant scan
Look for refactored predecessors and built-but-unused systems:

```bash
find . -path "*_archive*" -name "*.tsx"

# For each suspect file, count how many places it's rendered:
for c in OnboardingTour WelcomeWizard FirstRunGuide; do
  count=$(grep -rln "$c" app/ | wc -l)
  echo "$c: $count renders"
done
```

A file with zero renders is built-but-dormant — present in the repo, invisible to the user.

### Vector 5 — Sibling render-tree comparison
Read the layout where the feature *would* live, and read its siblings. The diff between what each wraps is the gap. If `DashboardLayout` mounts `AriaPresenceProvider` and `OnboardingLayout` doesn't, that's the system you missed.

## Reporting format

Don't say "this isn't built." Say:

> Checked vectors 1–5 for `OnboardingTour`. Vector 1: no exact match. Vector 2: found `WelcomeFlow` in `components/onboarding/welcome-flow.tsx`. Vector 3: directory exists with 4 files. Vector 4: `WelcomeFlow` has 0 renders — built but dormant. Vector 5: `OnboardingLayout` doesn't mount it. Confidence: this was built, deactivated, and forgotten.

That report is decision-grade. "It isn't built" is not.

## Anti-pattern

Running vector 1, finding nothing, declaring the feature absent. This is the most common version of the failure and the most expensive — because the next step is usually "let's build it," which means a duplicate implementation that has to be reconciled later.

## Heuristic

If you haven't run vector 4 (archive + dormant scan), you don't know the codebase well enough to claim something is missing. The codebase has more in it than you think it does.

## Related

- [03. Cold-context reviewer](./03-cold-context-reviewer.md) — the principle of not trusting your first pass.
