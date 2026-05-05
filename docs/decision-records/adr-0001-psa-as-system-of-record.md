# ADR-0001: ConnectWise PSA is the system of record for tickets, agreements, and customer state

**Status:** Accepted

## Context
The MSP operates dozens of integrated systems — RMM, EDR/MDR, identity provider, distributor portal, network monitoring, documentation tool, backup. Each one has its own concept of "customer", its own concept of "asset", its own concept of "current state". When the same fact is represented in multiple systems, it drifts.

Without a designated system of record, every automation has to make its own decision about which source to trust, and those decisions silently disagree. A workflow reading "is this customer active?" from the RMM gets a different answer than one reading from the distributor; both differ from what the PSA says; the SOC has yet another view.

## Decision
**ConnectWise PSA is the single source of truth for:**

- Ticket state — every ticket lives in the PSA. Tickets created by other systems are propagated *into* the PSA, not held externally.
- Agreement state — managed-services agreements, SLA tiers, contracted quantities, renewal dates.
- Customer state — whether a customer is currently managed, the contract type, the assigned TAM.

Other systems are sources of operational data (devices, agents, alerts, sign-ins, license assignments) but the PSA is the source of *business* truth.

Automations that need to know "is this customer ours?" or "what does this customer pay for?" read from the PSA. Automations that need to know "what's actually deployed at this customer?" read from the operational tools and reconcile against the PSA.

## Alternatives considered
- **No designated system of record.** Each automation chooses. Rejected — this is the status quo at most MSPs and produces the silent-drift problem we're trying to avoid.
- **Documentation tool (e.g. IT Glue) as system of record.** Rejected — it's a documentation surface, not a transactional one. Updates lag operational reality and the API surface is weaker for write operations.
- **A custom internal data layer above all the tools.** Rejected — the maintenance cost is high, the PSA already has most of the schema we need, and "build a CMDB" is a project we don't want to be in.

## Consequences

### Positive
- Every automation has a clear answer to "where does customer state come from?"
- Reconciliation patterns have a fixed reference point — they compare the operational tools *against* the PSA.
- TAMs already work in the PSA, so the system of record matches where the business decisions actually happen.
- ConnectWise is widely-deployed across MSPs, which makes the patterns in this catalogue more portable to other shops with the same setup.

### Negative
- The PSA's data model isn't perfect for everything we ask of it. Some fields are repurposed; some constraints don't fit; some operations are slow.
- We're reliant on ConnectWise's API surface and rate limits. When their API has issues, our automation has issues.
- Migrating away from ConnectWise (if that ever happened) would require revisiting every automation that treats the PSA as authoritative.

### Trade-offs we accept
- Some fields in the PSA are non-obvious or under-documented. We accept the cost of carrying internal documentation about how we use the schema rather than fighting the schema.
- ConnectWise rate limits constrain how aggressively recon patterns can run. We accept this as a hard ceiling and design recon cadences around it (mostly weekly, off-hours).

## Related patterns
- All patterns in this repo touch the PSA. The directly affected ones are everything in [`patterns/psa-ticketing/`](../../patterns/psa-ticketing/), the recon patterns in [`patterns/reconciliation/`](../../patterns/reconciliation/), and the finance/agreement patterns in [`patterns/finance-billing/`](../../patterns/finance-billing/).
