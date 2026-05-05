# Patterns by Category

The same patterns indexed differently — by category, with the dependency chain made explicit. If a pattern depends on another that's listed first, you should generally build the dependency first.

`*(coming)*` patterns will land in v0.2+. See [`../ProjectSpec.md`](../ProjectSpec.md) §9.

---

## platform-ops (build first)

The foundations every other pattern depends on.

| Pattern | Depends on |
|---|---|
| [Error Handling with Automated Ticket Creation](../patterns/platform-ops/error-handling-with-ticket-creation.md) | — |
| Logging and observability for workflows *(coming)* | error-handling |
| Production-readiness review *(coming)* | error-handling, logging |
| Secure Azure customer-tenant authentication *(coming)* | — |

## reconciliation foundations

| Pattern | Depends on |
|---|---|
| Organization & site mapping across platforms *(coming)* | error-handling |
| [RMM ↔ EDR ↔ Identity Agent Recon](../patterns/reconciliation/rmm-edr-agent-recon.md) | organization-mapping |
| [Offboarded Client System RECON](../patterns/reconciliation/offboarded-client-recon.md) | organization-mapping |

## psa-ticketing

| Pattern | Depends on |
|---|---|
| [Ticket Agreement Matcher](../patterns/psa-ticketing/ticket-agreement-matcher.md) | error-handling |
| [Catchall Company Name Remap](../patterns/psa-ticketing/catchall-company-name-remap.md) | error-handling |
| Auto-close cancelled tickets *(coming)* | error-handling |
| Stale ticket daily digest *(coming)* | error-handling |
| Ticket portal access form *(coming)* | error-handling |
| Recurring-issue detection *(coming)* | error-handling |
| ConnectWise webhook callback filtering *(coming)* | error-handling |

## identity-access

Higher-risk; build platform-ops + reconciliation foundations first.

| Pattern | Depends on |
|---|---|
| GDAP customer-tenant authentication *(coming)* | error-handling |
| [Self-Service Password Reset](../patterns/identity-access/self-service-password-reset.md) | error-handling |
| [RBAC Request with Conditional Access Automation](../patterns/identity-access/rbac-request-with-conditional-access.md) | error-handling, GDAP-auth |
| Employee onboarding *(coming)* | error-handling, GDAP-auth |
| Employee offboarding *(coming)* | error-handling, GDAP-auth |

## security-alerting

| Pattern | Depends on |
|---|---|
| [EDR Alert Enrichment with Sign-In Logs](../patterns/security-alerting/huntress-alert-enrichment.md) | error-handling, organization-mapping, GDAP-auth |
| SentinelOne quarantine-file alerts *(coming)* | error-handling |
| SentinelOne default-site endpoint cleanup *(coming)* | error-handling, organization-mapping |
| RMM recurring-alert dedup *(coming)* | error-handling |

## license-cost

| Pattern | Depends on |
|---|---|
| [M365 ↔ Pax8 ↔ PSA License Reconciliation](../patterns/license-cost/m365-pax8-psa-license-recon.md) | error-handling, organization-mapping, GDAP-auth |
| [Unused M365 License Detection](../patterns/license-cost/unused-m365-license-detection.md) | error-handling, GDAP-auth |
| NCE renewal automation *(coming)* | error-handling, GDAP-auth |
| NCE upcoming-license + user mapping *(coming)* | error-handling, GDAP-auth |
| Inactive product feedback loop *(coming)* | error-handling |

## azure-governance

| Pattern | Depends on |
|---|---|
| [Azure Reserved Instance Utilization Check](../patterns/azure-governance/azure-ri-utilization-check.md) | error-handling, secure-azure-auth |
| [Azure Proactive Resource Audit](../patterns/azure-governance/azure-proactive-resource-audit.md) | error-handling, secure-azure-auth |
| Azure backup monitoring *(coming)* | error-handling, secure-azure-auth |
| AVD nightly host-pool rebuild *(coming)* | error-handling |
| Customer-tenant backup recon *(coming)* | error-handling, secure-azure-auth |

## patch-lifecycle

| Pattern | Depends on |
|---|---|
| [RMM OS Patch Retry](../patterns/patch-lifecycle/rmm-os-patch-retry.md) | error-handling |
| Software patch retry *(coming)* | error-handling |
| Windows 11 upgrade campaign *(coming)* | error-handling |
| EOL/EOS lifecycle tagging *(coming)* | error-handling |

## reporting

Reporting patterns lean on the data the operational patterns produce. Build the operational patterns first.

| Pattern | Depends on |
|---|---|
| [Eight-Hour Ticket Activity Digest](../patterns/reporting/eight-hour-ticket-digest.md) | error-handling |
| Stale ticket daily digest *(coming)* | error-handling |
| Service desk timesheet daily HTML report *(coming)* | error-handling |
| Solutions engineer timesheet daily HTML report *(coming)* | error-handling |
| Password-reset audit report *(coming)* | error-handling, password-reset |
| OS patch compliance report *(coming)* | error-handling |
| IT-managed-services ticket reporting *(coming)* | error-handling |

## finance-billing

| Pattern | Depends on |
|---|---|
| [Contract Expiration Automation](../patterns/finance-billing/contract-expiration-automation.md) | error-handling |
| Annual price increase notification *(coming)* | error-handling |
| Inactive additions on active agreements *(coming)* | error-handling |
| Non-billable time conversion *(coming)* | error-handling |
| Credit-hold release on payment *(coming)* | error-handling |
| Agreement-count reconciliation *(coming)* | error-handling |

---

## Suggested build order (short version)

1. **platform-ops** — error-handling first. Without it, every later pattern is fragile.
2. **organization mapping** — once. Almost every later pattern depends on it.
3. **psa-ticketing hygiene wins** — agreement matcher, catchall remap, auto-close. Cheap, visible, reduce service-desk noise.
4. **reconciliation** — RMM/EDR/identity recon. The trust-but-verify backbone.
5. **license-cost** — pays for the program in year one.
6. **identity-access** — high user-visible value. Get error-handling and GDAP solid first.
7. **reporting** — once the data exists, automate the reports.
8. **finance-billing** — closes the loop with the agreement and invoice side.
9. **azure-governance** — ongoing customer optimization.
10. **patch-lifecycle** — tail-trimming on top of existing RMM workflows.

A more complete maturity model lands in `../docs/maturity-model.md` in v0.3.
