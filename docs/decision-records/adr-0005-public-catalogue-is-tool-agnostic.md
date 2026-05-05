# ADR-0005: The public pattern catalogue describes patterns tool-agnostically

**Status:** Accepted

## Context
This repository serves a different audience than our internal documentation. Internal docs describe how a workflow is built in a specific orchestrator with specific node configurations against specific accounts. The public catalogue describes the *pattern* — the problem, the trigger, the data flow, the outcome — in a way that's portable across orchestrators.

Two reasons matter:

1. **Audience reach.** Other MSPs run different orchestrators. Some are on Rewst, some on n8n, some on Power Automate, some on Make, some hand-rolled. A catalogue that's bound to a single platform talks past the majority of the audience.
2. **Pattern durability.** Orchestrators come and go. The patterns themselves — recon, hygiene, license drift detection, identity lifecycle — are durable across orchestrator generations. We've already lived through one orchestrator transition (see [ADR-0002](./adr-0002-default-orchestrator-is-n8n.md)); the pattern descriptions should outlast the next one too.

There's a secondary editorial concern: a public-repo catalogue that reads like marketing for one specific platform is less credible than one that reads like vendor-neutral practitioner notes.

## Decision
**Pattern cards in this repository describe automations tool-agnostically.**

- The `Orchestrator` field on each pattern card defaults to `tool-agnostic`, with an optional parenthetical noting where the production version was built (e.g. `tool-agnostic (originally Rewst)`).
- Flow shapes are described as logical steps, not as node sequences.
- Mermaid diagrams use generic labels ("PSA", "RMM", "Workflow") rather than orchestrator-specific node types.
- Implementation notes that are genuinely orchestrator-specific (e.g. "this only works because of an n8n code-node escape hatch") appear as gotchas or footnotes, never as the lede.
- Pattern names don't reference the orchestrator unless the pattern is fundamentally orchestrator-specific (rare; we haven't published one yet).

This is an editorial standard enforced through the [pattern card template](../pattern-card-template.md) and the [Rules.md §7 review checklist](../../Rules.md).

## Alternatives considered
- **Document patterns with orchestrator-specific implementation details.** Rejected — narrows the audience and dates the content quickly.
- **Maintain a parallel n8n-specific catalogue.** Rejected — doubles the maintenance burden, dilutes the editorial focus, and incentivizes leakage of internal implementation details into a public artifact.
- **Publish workflow exports for each pattern.** Explicitly rejected — see [`Rules.md`](../../Rules.md) §3.3. Exports leak credentials, environment-specific config, and internal identifiers, and they're orchestrator-specific by definition.

## Consequences

### Positive
- The catalogue is useful to MSPs on any orchestrator. The patterns translate.
- Editorial bar is consistent — a reader can compare patterns across categories without context-switching between platform vocabularies.
- The repo's credibility benefits from reading as practitioner notes rather than vendor marketing.
- Patterns survive orchestrator transitions; we don't need to rewrite the catalogue every time the underlying tooling shifts.

### Negative
- An MSP on a specific platform has to do their own translation step from "the pattern" to "the implementation in tool X." Some readers will want more concrete implementation guidance than the patterns provide.
- The temptation to slip in orchestrator-specific implementation details — particularly during the translation from internal to public — is real and has to be policed.
- Some genuinely useful platform-specific tricks won't appear here because they don't fit the editorial standard. We accept the loss.

### Trade-offs we accept
- The patterns are necessarily abstract. Readers who want a deeper "and here's how to wire it up in n8n" treatment have to ask, build, or look elsewhere. We're comfortable with that — the catalogue's job is to compress decision-making and prior art, not to be a tutorial.
- Editorial discipline is required on every pattern PR. The cost is modest but ongoing.

## Related artifacts
- [Pattern card template](../pattern-card-template.md) — encodes the tool-agnostic structure.
- [Rules.md §7](../../Rules.md) — review checklist that enforces it.
- [ADR-0002](./adr-0002-default-orchestrator-is-n8n.md) — internal default orchestrator (n8n) is a private decision; the public catalogue stays neutral.
