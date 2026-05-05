# AP-0003: Don't bulk-operate on customer tenants without CSP/PSA recon

**Pattern category:** identity-access

## What we tried
Rolled out a customer-tenant authentication change (a GDAP-based access flow) across the full managed customer base in a single push. The plan: walk the customer list from the PSA, request consent in each tenant, flip the workflow to the new auth model when consent succeeded.

## Why it didn't work
The success rate was roughly 50%. Roughly half the failures had a single root cause: customers who were no longer under management — long-since offboarded — but who were *still in the CSP relationship* on the partner-center side. The PSA said "this customer is gone"; the CSP said "this customer is yours". The workflow attempted consent in tenants the customer no longer wanted us in, against accounts that no longer existed, with predictable results.

The failures were noisy, alarming, and largely orthogonal to whether the new auth model was correct.

## What we did instead
Stopped the rollout. Built a CSP-vs-PSA reconciliation pass: for every customer in the CSP relationship, confirm they're still in the PSA's managed-customer list, and vice versa. Drove the cleanup back through the offboarding team, who removed the stale CSP relationships. Resumed the rollout against a clean customer set; the success rate jumped accordingly.

## Lesson
Customer-tenant identity has multiple sources of truth (PSA, CSP, RMM, EDR — pick at least three) and they drift quietly. Bulk operations against one source are guaranteed to surface the drift loudly.

The right sequence is always: **reconcile first, bulk-operate second.** Run the recon job, fix the gaps, then run the bulk operation against the reconciled set. The recon job is also the artifact you want when something goes wrong — it tells you whether you're looking at a real failure or stale state.

This is also a good argument for never deleting state from the recon report just because it looks resolved. The historical record is what tells you whether a customer was offboarded cleanly six months ago or just disappeared from one system.

## Related patterns
- [RMM ↔ EDR ↔ Identity Agent Recon](../../patterns/reconciliation/rmm-edr-agent-recon.md)
- [Offboarded Client System RECON](../../patterns/reconciliation/offboarded-client-recon.md)
- *(future)* GDAP customer-tenant authentication
