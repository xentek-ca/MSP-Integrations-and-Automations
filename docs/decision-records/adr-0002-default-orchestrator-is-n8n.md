# ADR-0002: Default orchestrator for new builds is n8n; legacy Rewst flows migrate opportunistically

**Status:** Accepted

## Context
The first wave of automation work was built on Rewst, an MSP-targeted RPA platform. Rewst was the right choice at the time — it shipped with first-class connectors for the MSP stack (PSA, RMM, EDR, distributor) and a low-code interface that lowered the bar to the first dozen workflows.

As the program matured, several constraints of the chosen platform started to dominate:

- **Tied to a single vendor's roadmap.** Features we needed were on the vendor's backlog or behind tier upgrades. We were paying retail for the platform's pace.
- **Limited code-side escape hatches.** Patterns that needed real programmatic control (custom auth flows, complex data transformations, cross-tenant operations) were possible but awkward.
- **Cost model.** Per-execution-style billing scaled badly as the workflow count grew, particularly for the high-frequency reconciliation and reporting patterns.
- **Hosting model.** No on-premises option meant we couldn't keep certain workflow execution in-house when that was the right answer for compliance or sensitivity reasons.

n8n addressed each of these: open-core, JavaScript code-node escape hatches, predictable hosting cost, and an on-prem deployment option. The connector library is less MSP-specific than Rewst's, but the gap is closeable in JavaScript and we now have a body of integration code to reuse.

## Decision
**New automation work is built on n8n by default.**

- New patterns are implemented in n8n unless there's a specific reason not to.
- Existing Rewst flows are not auto-migrated. Migration happens opportunistically — when a flow needs significant work anyway (a feature addition, a sanitization fix, a vendor-API change), we evaluate whether to rebuild it in n8n at the same time.
- Both platforms remain in production indefinitely. The transition is gradual and governed by what's economically sensible, not by a deadline.
- The public pattern catalogue is documented tool-agnostically (see ADR-0005) so neither platform is privileged in the patterns themselves.

## Alternatives considered
- **Stay on Rewst exclusively.** Rejected — the constraints above accumulate and the ceiling is real.
- **Migrate everything to n8n on a schedule.** Rejected — migration is expensive, the existing Rewst flows work, and there's no business pressure forcing a hard cutover. Opportunistic migration is the right pace.
- **Move to Power Automate.** Rejected — strong Microsoft-side bindings but weak everywhere else in the MSP stack; cost model is opaque; the dev/test loop is slow.
- **Move to Make (formerly Integromat).** Rejected — capable, but the MSP-stack connectors are weaker and the cost model has the same shape as Rewst.
- **Build on a code framework (Temporal, Airflow, custom).** Rejected as the *primary* platform — too much investment in primitives that orchestrators already provide. Used selectively for specific workloads where the orchestrator isn't the right fit.

## Consequences

### Positive
- New workflow builds get the full power of code-node escape hatches when they need it.
- Cost ceiling is much higher; high-frequency workflows aren't economically constrained.
- On-prem deployment unlocks workloads that need to stay in-house (see [ADR-0003](./adr-0003-n8n-on-prem.md)).
- Less coupling to any single vendor's roadmap.

### Negative
- Two orchestrators in production means two sets of credentials, two error-handling implementations to keep aligned, two deploy/CI surfaces, two skill sets.
- n8n's MSP-stack connectors are thinner than Rewst's; we're investing in shared integration code to make up the gap.
- Rewst-side flows are a sunk-cost trap if we let them sit indefinitely. Opportunistic migration discipline is required.

### Trade-offs we accept
- We pay the cost of running two orchestrators during the transition. The transition has no deadline, and we accept that some Rewst flows may stay in production for years.
- The internal team needs working knowledge of both platforms, which constrains hiring and ramps.

## Related decisions
- [ADR-0003](./adr-0003-n8n-on-prem.md) — Run n8n on-premises rather than n8n cloud.
- [ADR-0005](./adr-0005-public-catalogue-is-tool-agnostic.md) — The public catalogue is tool-agnostic.
