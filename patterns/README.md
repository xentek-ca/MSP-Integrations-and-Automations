# Patterns Index

Two ways to navigate:

- **By category** — folders below, each with a category description and the patterns inside it.
- **By MSP tool** — see [`../taxonomy/tools.md`](../taxonomy/tools.md).

Patterns labeled `*(coming)*` are referenced by published patterns but haven't been written up yet. They'll land in v0.2 and beyond — see [`../ProjectSpec.md`](../ProjectSpec.md) §9.

---

## PSA / Ticketing

[`psa-ticketing/`](./psa-ticketing/)

Automation around the PSA — auto-assignment, agreement matching, status hygiene, ticket-portal access, recurring-issue detection. The category most MSPs start with because the PSA is where the team actually lives.

- [Ticket Agreement Matcher](./psa-ticketing/ticket-agreement-matcher.md) — auto-attach the right SLA-tier agreement to incoming tickets so time entries bill to the right contract.
- [Catchall Company Name Remap](./psa-ticketing/catchall-company-name-remap.md) — read incoming-ticket notes and re-company tickets parked on a catchall to the correct managed customer.

## Cross-platform reconciliation

[`reconciliation/`](./reconciliation/)

The trust-but-verify category. Devices and organizations span half a dozen platforms; recon catches drift between what should be deployed and what actually is.

- [RMM ↔ EDR ↔ Identity Agent Recon](./reconciliation/rmm-edr-agent-recon.md) — weekly cross-platform check that every managed device has an agent in every platform it's supposed to.
- [Offboarded Client System RECON](./reconciliation/offboarded-client-recon.md) — when a customer is offboarded in the PSA, sweep every platform and remove or flag their footprint.

## Identity, access, onboarding

[`identity-access/`](./identity-access/)

Lifecycle and access-control automations. The highest user-visible value, also the highest-risk-if-rushed.

- [Self-Service Password Reset](./identity-access/self-service-password-reset.md) — manager-initiated reset with hybrid detection (cloud-only via Graph; hybrid via on-prem PowerShell dispatch).
- [RBAC Request with Conditional Access Automation](./identity-access/rbac-request-with-conditional-access.md) — time-bounded elevated access via Conditional Access patches with email approval and automatic revert.

## Security & alerting

[`security-alerting/`](./security-alerting/)

Enrichment, dedup, and routing for the alerts the SOC actually sees.

- [EDR Alert Enrichment with Sign-In Logs](./security-alerting/huntress-alert-enrichment.md) — enrich incoming account-level EDR alerts with recent sign-in audit data on ticket creation.

## License & cost optimization

[`license-cost/`](./license-cost/)

The category that often pays for the entire automation program in year one.

- [M365 ↔ Pax8 ↔ PSA License Reconciliation](./license-cost/m365-pax8-psa-license-recon.md) — three-way weekly recon that catches license drift across what's billed, what's provisioned, and what's contracted.
- [Unused M365 License Detection](./license-cost/unused-m365-license-detection.md) — surface licenses assigned to disabled or dormant users plus over-purchased seats; produce a per-customer removal candidate list.

## Azure governance

[`azure-governance/`](./azure-governance/)

Cost, hygiene, and operational patterns for customer Azure subscriptions.

- [Azure Reserved Instance Utilization Check](./azure-governance/azure-ri-utilization-check.md) — weekly check that running VMs are RI-covered and existing RIs are 100% utilized.
- [Azure Proactive Resource Audit](./azure-governance/azure-proactive-resource-audit.md) — find unattached disks, orphaned snapshots, unassociated public IPs, idle VPN gateways, and stopped-but-not-deallocated VMs.

## Patch & lifecycle

[`patch-lifecycle/`](./patch-lifecycle/)

Closing the long tail on patch compliance and device lifecycle.

- [RMM OS Patch Retry](./patch-lifecycle/rmm-os-patch-retry.md) — re-dispatch OS patches to devices that failed the previous run and are now online.

## Reporting

[`reporting/`](./reporting/)

The reports leadership keeps asking for, automated.

- [Eight-Hour Ticket Activity Digest](./reporting/eight-hour-ticket-digest.md) — shift-aligned rolling rollup of ticket creates and closes on configured boards.

## Finance & billing hygiene

[`finance-billing/`](./finance-billing/)

Patterns that keep the agreement-side data clean and the renewal pipeline visible.

- [Contract Expiration Automation](./finance-billing/contract-expiration-automation.md) — daily detection of agreements without expiration dates plus proactive renewal-conversation tickets at a fixed runway.

## Platform & operations

[`platform-ops/`](./platform-ops/)

The foundations every other pattern depends on — error handling, logging, secure auth, change control. These are the platform-level investments that make production automation safe.

- [Error Handling with Automated Ticket Creation](./platform-ops/error-handling-with-ticket-creation.md) — the standard failure-handling pattern every production workflow uses; referenced as a dependency throughout this repo.
