# AP-0001: Don't compete with vendor-native automation

**Pattern category:** platform-ops (cross-cutting)

## What we tried
Built an in-house automation to upgrade EDR agents to the latest version on a schedule. Pulled the version inventory from the EDR API, compared it to the available release, dispatched upgrade jobs to lagging endpoints. Standard "we'll make this work despite the vendor" project.

## Why it didn't work
The vendor shipped a first-class version-management feature in their own portal partway through the build. Their version was tighter — better staging, better failure handling, integrated with their support model — because they own the agent codebase and we don't. Continuing to maintain our own version of the workflow would have meant racing the vendor's roadmap forever.

## What we did instead
Decommissioned the in-house workflow. Adopted the vendor's native upgrade automation. Repurposed the engineering effort into the parts of the EDR experience the vendor *doesn't* automate — alert enrichment, cross-platform agent reconciliation, default-site cleanup.

## Lesson
Watch your vendors' roadmaps. If the gap you're filling with custom automation is on the vendor's product backlog, your work is going to be obviated — sometimes faster than you expect. Build automation in the *negative space* of what your vendors do, not in direct overlap with it.

The corollary: build infrastructure that's portable across vendor versions (the org-mapping layer, the error-handling pattern, the reconciliation framework) rather than tightly-coupled wrappers around a single vendor feature.

## Related patterns
- [RMM ↔ EDR ↔ Identity Agent Recon](../../patterns/reconciliation/rmm-edr-agent-recon.md) — the kind of cross-vendor work no individual vendor will build for you.
- [EDR Alert Enrichment with Sign-In Logs](../../patterns/security-alerting/huntress-alert-enrichment.md) — enrichment with cross-tenant identity data; same observation.
