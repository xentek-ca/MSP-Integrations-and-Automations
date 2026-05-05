# ADR-0003: Run n8n on-premises rather than n8n cloud for production workloads

**Status:** Accepted

## Context
n8n is available as both a vendor-hosted SaaS (n8n cloud) and a self-hostable open-source product. The cloud offering is faster to start with — no infrastructure, no upgrades, no operational burden. The self-host option requires running the server, the database, the workers, and the upgrade lifecycle ourselves.

Several factors pushed us toward on-prem for production:

- **Customer-tenant authentication context.** A meaningful portion of our workflows touch customer Microsoft tenants under GDAP. Running these workflows in a vendor's cloud creates an additional party in the customer-trust chain that we'd rather not introduce.
- **Sensitive workflow inputs.** Workflows that handle credentials, customer-tenant tokens, secret rotation, or financial data have privacy and compliance implications when their execution data lives in a third-party SaaS. We hit this directly during the rollout — see [AP-0004](../anti-patterns/ap-0004-execution-data-on-auth-flows.md).
- **Cost predictability at scale.** Cloud per-execution pricing is fine for low-volume work and bites for high-frequency reconciliation patterns. On-prem is a fixed infrastructure cost.
- **Egress and network position.** Some integrations are easier to run when the orchestrator is on a network we control rather than going outbound from a SaaS to customer-facing endpoints.
- **Open-source guarantees.** On-prem keeps us on the open-source license terms and insulates us from cloud-side feature gating or pricing changes.

## Decision
**Production workflows run on a self-hosted n8n instance.**

- The on-prem instance is the production target for all new and migrated workflows.
- Development and exploratory work may use n8n cloud or local instances; promotion to production means deploying to on-prem.
- We document the on-prem architecture (deployment, upgrades, backups, secrets management, observability) as a first-class artifact in the internal repository, separate from the pattern catalogue.

## Alternatives considered
- **n8n cloud for production.** Rejected for the reasons above.
- **Hybrid: cloud for non-sensitive workflows, on-prem for sensitive ones.** Rejected — running two production environments doubles operational overhead, and the categorization of "sensitive" drifts over time.
- **Containerized n8n on a third-party PaaS (Heroku, Railway, Fly.io).** Rejected — convenience win, but the data-at-rest concerns aren't materially different from cloud.

## Consequences

### Positive
- Customer-tenant authentication and execution data stay inside our infrastructure.
- Cost is predictable and decoupled from execution count.
- We control upgrade timing — no surprise feature changes mid-quarter.
- The on-prem deployment is also a credential-isolation boundary, which is useful for change-control and disaster-recovery planning.

### Negative
- Operational burden is real. We own the server, the database, the worker scaling, the upgrade lifecycle, the backup strategy, the monitoring of the orchestrator itself.
- The on-prem footprint is another asset on the internal asset list — it has to be patched, secured, monitored, and recovered like any other production system.
- Setup time to first workflow in production is longer than cloud.

### Trade-offs we accept
- We pay the operational cost of running our own instance to keep workflow execution in our trust boundary. That cost is real and ongoing.
- We commit to a maintenance discipline (regular upgrades, backups tested, monitoring on the orchestrator itself) that has to be funded.
- Some niceties of the cloud offering (e.g. some managed connector updates) require manual handling on our side.

## Related decisions
- [ADR-0002](./adr-0002-default-orchestrator-is-n8n.md) — Default orchestrator for new builds is n8n.
- [ADR-0004](./adr-0004-offboarding-recommends-not-deletes.md) — Offboarding automations recommend rather than delete.

## Related anti-patterns
- [AP-0004](../anti-patterns/ap-0004-execution-data-on-auth-flows.md) — Why execution-data persistence on auth-touching workflows pushed us toward on-prem.
