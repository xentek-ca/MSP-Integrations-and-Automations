# AP-0007: Not all SKU/identifier mapping is worth automating

**Pattern category:** license-cost

## What we tried
Scoped a workflow to automatically map a distributor's product UUIDs to the corresponding ConnectWise PSA product records, so that the agreement-additions side could be kept in lock-step with the distributor's catalogue without manual intervention. The intuition was reasonable: SKU normalization is mechanical and shows up across multiple license patterns, so automating it should pay back across all of them.

## Why it didn't work
The cost-benefit collapsed under inspection:

- The distributor catalogue and the PSA product catalogue are owned by different teams (procurement vs. finance). The "right" mapping is a *business* decision, not a mechanical one — what the SKU is *called* internally, what agreement type it lives under, what tax category applies, are decisions made deliberately at PSA-product-creation time and aren't reliably derivable from the distributor record.
- The distributor catalogue updates more frequently than the PSA catalogue *should*. Auto-syncing every catalogue change would have polluted the PSA with one-off SKUs that the finance team didn't actually want represented as agreement additions.
- The total volume of new SKUs needing mapping in any given month was small. The automation would have spent more orchestration cycles checking "is anything new?" than the manual mapping work it was meant to replace.
- An accurate translation table — owned by a human, edited deliberately, version-controlled — turned out to be a better artifact than an automation. It's reviewable, it's auditable, and it forces the conversation about *why* a given mapping exists when it gets touched.

## What we did instead
Marked the work as "Won't Do." Maintained the SKU translation table by hand as a first-class artifact, owned by the team that runs the recon patterns. Documented the table's owner, its review cadence, and the change-control process for adding new mappings.

The license-reconciliation patterns — which is what the mapping serves — read from that translation table. The table is the contract; how it gets maintained is a separate question.

## Lesson
Some translation/mapping problems look mechanical from the outside but are actually business-policy problems wearing a mechanical disguise. Automating them encodes whatever policy you happened to assume on the day you wrote the code, which then quietly drifts from the policy the business actually wants.

A useful filter: if the right answer to "should this entity exist?" requires a judgment call from someone who isn't on the automation team, the mapping isn't a candidate for automation — it's a candidate for a maintained, auditable table.

A second filter: small-volume work isn't always worth automating even when it's mechanical. The maintenance cost of a workflow has a floor; if the manual work below the floor is small enough, leaving it manual is the right answer. Automation isn't the goal; *predictable, low-toil operations* are the goal.

## Related patterns
- [M365 ↔ Pax8 ↔ PSA License Reconciliation](../../patterns/license-cost/m365-pax8-psa-license-recon.md) — the pattern that consumes the translation table.
- [Unused M365 License Detection](../../patterns/license-cost/unused-m365-license-detection.md)
