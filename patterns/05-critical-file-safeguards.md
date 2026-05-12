# 05. Critical-file safeguards

**Principle.** Some files are load-bearing enough that any edit needs a backup, a deletion count, and a second look. The list is short. The discipline is non-negotiable.

## Why

Most files in a codebase can be safely overwritten, reverted, or rewritten. A few cannot. Auth middleware, payment webhooks, database migrations, environment config — when one of these breaks, production breaks, and the blast radius is paying customers.

The expensive incident I learned this from: an agent edited the auth middleware to "clean up unused imports" and silently removed 667 lines of route protection logic that looked unused but were actually wired into the checkout flow. Checkout broke in production for hours.

The fix isn't to forbid edits to those files. It's to slow down the edits.

## The protocol

For every file on the critical list, before any edit:

```bash
# 1. Read the file in full
cat path/to/critical-file.ts

# 2. Count what the planned edit would delete
git diff path/to/critical-file.ts | grep "^-" | wc -l

# 3. If deletions > 50 → STOP. Get explicit approval before proceeding.
# 4. Create a backup
cp path/to/critical-file.ts path/to/critical-file.ts.backup

# 5. Apply the edit
# 6. Verify the file still parses / typechecks
```

The deletion-count threshold is the heart of the protocol. Adding lines is rarely catastrophic. Removing a large block of lines, almost always.

## The list

The exact list is project-specific, but the categories generalize:

- **Auth middleware.** Anything that decides who gets to access what.
- **Payment / billing handlers.** Stripe webhooks, subscription logic, refund flows.
- **Database migrations.** Once applied, hard to reverse.
- **Build / framework config.** `next.config.js`, `tsconfig.json`, `package.json`.
- **Environment files.** `.env.*` — never edit without reading first; never commit without scanning for secrets.
- **Anything documented as "do not touch."** Even — especially — if the documentation looks stale.

A short list is more useful than a long one. If everything is critical, nothing is.

## Anti-pattern

"It looked unused so I deleted it." The middleware incident, in one sentence. The reason something looks unused is often that you don't yet understand what uses it.

## Heuristic

If you wouldn't edit this file at 2am on a Friday with the on-call phone in the other hand, it's on the critical list. Treat it that way during a quiet Tuesday afternoon too.

## Related

- [06. Autonomy gradient](./06-autonomy-gradient.md) — these files sit at the "hard stop, always ask" end of the autonomy spectrum.
