# AP-0006: Vendor APIs drift silently — instrument every integration with a heartbeat

**Pattern category:** platform-ops

## What we tried
Treated MSP-stack APIs (RMM, EDR, PSA, identity) as stable contracts. Built integrations that assumed the API surface would remain consistent across versions — same endpoints, same response shapes, same authentication. Did our own end-to-end smoke test at build time and trusted the integration thereafter.

## Why it didn't work
Vendor APIs change. Sometimes the changes are announced; sometimes they're announced in a release note nobody on the automation team reads; sometimes they're shipped without being announced at all. We saw all three:

- Field renames in response payloads that broke downstream parsing.
- Authentication-header changes that started rejecting our (still valid) tokens.
- Pagination defaults that quietly halved the per-page size, so workflows that had been silently under-fetching for weeks were now under-fetching by half again.
- Endpoints deprecated in favor of new ones, with the old endpoint serving a stale subset of the data while still returning a 200.

The silent partial failures were the worst. A workflow that fails loudly is easy to fix; a workflow that fails *quietly* — returning fewer rows, returning slightly stale data, missing one field — accumulates incorrect downstream decisions for weeks before anyone notices.

## What we did instead
Added per-integration heartbeats. Each external integration we depend on gets a small, scheduled workflow whose job is to make a known-shape request, validate the response shape against an explicit schema, and ticket on any deviation. The heartbeat is independent of the production workflows that use the integration — its only purpose is to detect drift.

Schema validation specifically catches the renames-and-removals class of failure. Response-row-count baselines catch the pagination-style silent failures. Both are short, low-effort workflows, and they pay for themselves the first time they catch something.

## Lesson
Don't trust vendor API stability. Assume APIs drift; design for detection. The cheapest possible insurance is a heartbeat workflow per integration that exercises the contract you depend on and fails loudly when the contract changes.

A practical check: for every external system your automation depends on, can you answer the question "if the response shape changed tomorrow, when would we find out?" If the answer is "the next time a customer complains", you need a heartbeat.

## Related patterns
- [Error Handling with Automated Ticket Creation](../../patterns/platform-ops/error-handling-with-ticket-creation.md) — the heartbeat pipes into the same failure-ticketing path as everything else.
- *(future)* Logging and observability for workflows.
- *(future)* Integration heartbeat pattern.
