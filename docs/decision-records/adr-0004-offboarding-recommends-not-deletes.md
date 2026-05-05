# ADR-0004: Offboarding automations recommend and ticket; humans confirm destructive actions

**Status:** Accepted

## Context
A meaningful share of MSP automation work is destructive — removing a user, decommissioning a device, deleting a subscription, archiving a tenant, deactivating a license. Destructive automation is high-leverage when it works and disproportionately bad when it doesn't. A wrongly-applied delete is sometimes recoverable from backup; sometimes it isn't.

Specifically for offboarding work — both customer offboarding and employee offboarding — the workflows touch every system in the stack. There is no realistic way to test an offboarding workflow against the actual data it will operate on without putting real customer state at risk.

The trade-off is between operational speed (auto-delete fast and consistently) and recoverability (ticket and let humans confirm). The cost of a wrong auto-delete in this category is much higher than the cost of a slightly slower offboarding.

## Decision
**Offboarding automations recommend and create tickets for destructive actions; they do not auto-delete.**

- The pattern detects what should be removed, generates structured cleanup tasks (one per platform requiring action), and writes them to the offboarding ticket as a checklist.
- For platforms where API-driven removal is supported, the workflow may queue the removal as a *staged* action — created in a draft / pending state where supported, or noted as "ready to execute" where not — but the actual destructive call is gated on human confirmation.
- For platforms where removal can be done via API safely *and* the action is reversible with low cost (e.g. revoking an API token), the workflow may proceed automatically, with the action logged on the offboarding ticket.

The default is "recommend, ticket, wait for confirmation". Exceptions to that default are explicit and per-platform.

## Alternatives considered
- **Full auto-delete on offboarding.** Rejected — the failure mode (accidentally destroying state for a customer who isn't actually being offboarded, or destroying more state than intended) is unrecoverable. The speed-up doesn't justify the risk profile.
- **Manual offboarding only, no automation.** Rejected — the manual process is exactly what the automation is meant to replace, and the manual process has its own (different, but real) error mode of *missing* steps.
- **Auto-delete behind a per-platform feature flag.** Rejected as a default — it inverts the right safety bias. The flag should be "yes, automate this destructive action" and the absence of the flag should mean "ticket and wait", not the other way around.

## Consequences

### Positive
- Offboarding is faster and more consistent than the manual baseline because the cross-platform recon work is automated even when the destructive step isn't.
- The audit trail is strong — every destructive action that happens, happens because a human approved it on a specific ticket.
- The workflow's failure mode is "human gets a checklist", which is the same failure mode as manual offboarding minus the "I forgot a system" error.

### Negative
- Offboarding takes longer than it would if everything were auto-deleted. The cleanup window for stragglers is days or weeks, not minutes.
- Humans have to actually action the checklist. Discipline matters; if the offboarding-ticket queue gets ignored, the cleanup doesn't happen.
- Some platforms with weak APIs require purely manual cleanup, and the automation can only document the requirement.

### Trade-offs we accept
- We accept a slower offboarding clock in exchange for a sharper safety property. Accidentally-destroyed state is, in practice, the worst outcome class in this category, and we design against it explicitly.
- We accept that the offboarding queue is a real operational responsibility someone has to own. The automation reduces toil; it doesn't eliminate the human role.

## Related patterns
- [Offboarded Client System RECON](../../patterns/reconciliation/offboarded-client-recon.md) — implements this principle for customer offboarding.
- *(future)* Employee offboarding pattern — same principle.
