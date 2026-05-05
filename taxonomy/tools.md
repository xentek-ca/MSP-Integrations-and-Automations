# Patterns by MSP Tool

Pick your stack, see the patterns. Patterns appear under every tool they meaningfully touch — a pattern that reads from ConnectWise PSA *and* writes to it appears under that tool once.

`*(coming)*` patterns are referenced by published patterns and will land in v0.2+. See [`../ProjectSpec.md`](../ProjectSpec.md) §9.

---

## ConnectWise PSA

The hub of most MSP automation. Almost every pattern touches it.

- [Ticket Agreement Matcher](../patterns/psa-ticketing/ticket-agreement-matcher.md)
- [Catchall Company Name Remap](../patterns/psa-ticketing/catchall-company-name-remap.md)
- [RMM ↔ EDR ↔ Identity Agent Recon](../patterns/reconciliation/rmm-edr-agent-recon.md)
- [Offboarded Client System RECON](../patterns/reconciliation/offboarded-client-recon.md)
- [RBAC Request with Conditional Access Automation](../patterns/identity-access/rbac-request-with-conditional-access.md)
- [EDR Alert Enrichment with Sign-In Logs](../patterns/security-alerting/huntress-alert-enrichment.md)
- [M365 ↔ Pax8 ↔ PSA License Reconciliation](../patterns/license-cost/m365-pax8-psa-license-recon.md)
- [Unused M365 License Detection](../patterns/license-cost/unused-m365-license-detection.md)
- [Azure Reserved Instance Utilization Check](../patterns/azure-governance/azure-ri-utilization-check.md)
- [Azure Proactive Resource Audit](../patterns/azure-governance/azure-proactive-resource-audit.md)
- [Eight-Hour Ticket Activity Digest](../patterns/reporting/eight-hour-ticket-digest.md)
- [Contract Expiration Automation](../patterns/finance-billing/contract-expiration-automation.md)
- [Error Handling with Automated Ticket Creation](../patterns/platform-ops/error-handling-with-ticket-creation.md)

## NinjaOne (NinjaRMM)

- [RMM ↔ EDR ↔ Identity Agent Recon](../patterns/reconciliation/rmm-edr-agent-recon.md)
- [Offboarded Client System RECON](../patterns/reconciliation/offboarded-client-recon.md)
- [Self-Service Password Reset](../patterns/identity-access/self-service-password-reset.md) (PowerShell dispatch for hybrid users)
- [RMM OS Patch Retry](../patterns/patch-lifecycle/rmm-os-patch-retry.md)

## SentinelOne

- [RMM ↔ EDR ↔ Identity Agent Recon](../patterns/reconciliation/rmm-edr-agent-recon.md)
- [Offboarded Client System RECON](../patterns/reconciliation/offboarded-client-recon.md)

## Huntress

- [RMM ↔ EDR ↔ Identity Agent Recon](../patterns/reconciliation/rmm-edr-agent-recon.md)
- [Offboarded Client System RECON](../patterns/reconciliation/offboarded-client-recon.md)
- [EDR Alert Enrichment with Sign-In Logs](../patterns/security-alerting/huntress-alert-enrichment.md)

## Liongard

- [Offboarded Client System RECON](../patterns/reconciliation/offboarded-client-recon.md)

## Auvik

- [Offboarded Client System RECON](../patterns/reconciliation/offboarded-client-recon.md)

## Microsoft 365 (Graph, Entra, Exchange)

- [RMM ↔ EDR ↔ Identity Agent Recon](../patterns/reconciliation/rmm-edr-agent-recon.md) (Entra-joined endpoints)
- [Self-Service Password Reset](../patterns/identity-access/self-service-password-reset.md)
- [RBAC Request with Conditional Access Automation](../patterns/identity-access/rbac-request-with-conditional-access.md)
- [EDR Alert Enrichment with Sign-In Logs](../patterns/security-alerting/huntress-alert-enrichment.md)
- [M365 ↔ Pax8 ↔ PSA License Reconciliation](../patterns/license-cost/m365-pax8-psa-license-recon.md)
- [Unused M365 License Detection](../patterns/license-cost/unused-m365-license-detection.md)

## Microsoft Azure (ARM)

- [Azure Reserved Instance Utilization Check](../patterns/azure-governance/azure-ri-utilization-check.md)
- [Azure Proactive Resource Audit](../patterns/azure-governance/azure-proactive-resource-audit.md)

## Microsoft Partner Center / CSP

- [EDR Alert Enrichment with Sign-In Logs](../patterns/security-alerting/huntress-alert-enrichment.md)
- [M365 ↔ Pax8 ↔ PSA License Reconciliation](../patterns/license-cost/m365-pax8-psa-license-recon.md)

## Pax8

- [M365 ↔ Pax8 ↔ PSA License Reconciliation](../patterns/license-cost/m365-pax8-psa-license-recon.md)

## Microsoft Teams

- [Self-Service Password Reset](../patterns/identity-access/self-service-password-reset.md) (delivery channel)

---

## Tools tagged in v0.2+ patterns (not yet published)

The following tools appear in the source backlog and will be tagged here when their patterns publish:

- **Citrix** — unregistered-warning ticket handling.
- **Dropsuite** — backup integration patterns.
- **Mimecast** — mail-flow integration touchpoints.
- **IT Glue** — documentation-side cleanup and lifecycle.
- **CIPP** — CSP-side reconciliation patterns.
