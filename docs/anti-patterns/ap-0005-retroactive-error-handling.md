# AP-0005: Retroactive error handling is more expensive than building it from day one

**Pattern category:** platform-ops

## What we tried
Built the first wave of production workflows without a consistent error-handling pattern. Each workflow had ad-hoc handling — some had try/catch around the risky steps, some had nothing, some had a half-built notification path that pointed to a Slack channel nobody read. The first wave shipped; the workflows worked; nobody had time to standardize error handling and the team moved on to the next wave.

The cost of this was hidden until enough workflows had been live for long enough that they started failing in production.

## Why it didn't work
Two failure modes accumulated:

1. **Silent failures** — a workflow stopped firing or started returning errors, and nobody noticed. The team trusted the workflow was running because it had been running. Mean-time-to-detect was "the next time someone happened to look".
2. **Inconsistent triage** — the workflows that *did* surface failures did so in inconsistent ways. One emailed; one Slacked; one created a low-priority ticket on the wrong board; one logged silently and was thrown away on next run. There was no single place to look when an automation broke.

Going back and retrofitting a consistent error-handling pattern across dozens of already-built workflows turned out to be more expensive than building it correctly the first time. Every workflow had to be re-tested against the new pattern; subtle behavioral changes (e.g. workflows that previously swallowed errors now propagated them) needed to be reviewed individually.

## What we did instead
Defined the standard error-handling pattern (now [`error-handling-with-ticket-creation`](../../patterns/platform-ops/error-handling-with-ticket-creation.md)). Made it a hard requirement of the production-readiness review — no workflow ships to production without it. Migrated existing workflows to the pattern over time, prioritized by how much state each workflow mutates externally.

Net effect, six months later: failures are visible the run they happen, mean-time-to-detect is the cron interval, and the engineering team has one queue to look at.

## Lesson
Error handling is platform infrastructure, not per-workflow polish. Build it as a shared service or shared pattern from day one. Every production workflow gets it, no exceptions, no "we'll add it later".

The deeper lesson: when you ship the first wave of workflows for an automation program, ship the platform-level patterns alongside them — error handling, structured logging, secure auth, sanitization. The workflows themselves are valuable; the patterns under them are what makes the program scale. Skipping the platform layer in the name of velocity is a debt that compounds.

If your automation team is at the "first dozen workflows" stage and doesn't have a standard error-handling pattern: stop shipping new workflows for two weeks and build one. It will pay for itself before the year is out.

## Related patterns
- [Error Handling with Automated Ticket Creation](../../patterns/platform-ops/error-handling-with-ticket-creation.md) — the standard we settled on.
- *(future)* Production-readiness review checklist.
- *(future)* Logging and observability for workflows.
