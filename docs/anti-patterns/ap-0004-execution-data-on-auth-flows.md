# AP-0004: Don't persist execution data on auth-touching workflows

**Pattern category:** platform-ops

## What we tried
Built a pair of workflows for centralized credential management — one to set secrets, one to retrieve them — using the orchestrator's default settings. Default settings included persisting full execution data (inputs, outputs, intermediate state) for every run, which is the right default for almost every other workflow because it's how you debug them.

## Why it didn't work
"Inputs and outputs" on an auth-touching workflow include the secret being set or retrieved. The orchestrator was storing customer-tenant credentials in its own execution history — a place those credentials should never have lived. The risk wasn't theoretical: anyone with read access to the orchestrator's run history had read access to those secrets.

We caught it in review before it became an incident. The default that was harmless on every other workflow was actively dangerous on these two.

## What we did instead
Disabled execution-data persistence on the auth-touching workflows specifically. Documented this as a required step in the production-readiness review for any new workflow whose inputs or outputs include credentials, tokens, or anything else marked sensitive. Added a code-review checkbox to enforce it.

## Lesson
Orchestrator defaults are tuned for debuggability, not for secrets handling. The same setting that makes 99% of your workflows easier to operate is a credential exfiltration risk on the 1% of workflows that touch auth.

The broader lesson: every orchestrator has settings that are correct as defaults but wrong for specific workflow classes. Build a checklist that surfaces these decisions explicitly during production-readiness review. Don't rely on the engineer remembering to disable execution-data on the right workflows; rely on the review.

A short list of orchestrator defaults to think twice about for production workflows:

- Execution-data retention — fine for debugging, dangerous for secrets.
- Webhook URLs — convenience, but anyone with the URL can trigger.
- Error notification routing — defaults often go to the workflow author, not the on-call.
- Retry-on-failure — convenient, but on a workflow that mutates external state, retries can double-write.

## Related patterns
- [Error Handling with Automated Ticket Creation](../../patterns/platform-ops/error-handling-with-ticket-creation.md) — sanitization of captured context is the same family of problem.
- *(future)* Production-readiness review checklist.
- *(future)* Secure customer-tenant authentication patterns.
