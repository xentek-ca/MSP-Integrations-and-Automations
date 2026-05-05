# Architecture Decision Records

The decisions that shape this automation program. These aren't theoretical positions — every ADR here records a call we actually made, with the context that prompted it and the trade-offs we accepted.

The format is a compressed version of Michael Nygard's classic ADR template:

- **Status** — Accepted, Superseded, or Deprecated.
- **Context** — what situation forced the decision.
- **Decision** — what we chose.
- **Alternatives considered** — what we explicitly rejected and why.
- **Consequences** — positive, negative, and the trade-offs we knowingly accepted.

ADRs are immutable. When a decision changes, the old ADR is marked Superseded and a new one is written that references it. The historical record is the point.

---

## Index

| ID | Title | Status |
|---|---|---|
| [ADR-0001](./adr-0001-psa-as-system-of-record.md) | ConnectWise PSA is the system of record for tickets, agreements, and customer state | Accepted |
| [ADR-0002](./adr-0002-default-orchestrator-is-n8n.md) | Default orchestrator for new builds is n8n; legacy Rewst flows migrate opportunistically | Accepted |
| [ADR-0003](./adr-0003-n8n-on-prem.md) | Run n8n on-premises rather than n8n cloud for production workloads | Accepted |
| [ADR-0004](./adr-0004-offboarding-recommends-not-deletes.md) | Offboarding automations recommend and ticket; humans confirm destructive actions | Accepted |
| [ADR-0005](./adr-0005-public-catalogue-is-tool-agnostic.md) | The public pattern catalogue describes patterns tool-agnostically | Accepted |
