# MSP Automation Patterns

A public, sanitized catalogue of automation patterns built and run in production across the standard MSP stack — ConnectWise PSA, NinjaOne, SentinelOne, Huntress, Liongard, Auvik, Microsoft 365 / Entra, Azure, Pax8, and adjacent tools.

Published by **Xentek**.

---

## What this is

This repo describes — at the *pattern* level — automations a working MSP has built, tested, and put in front of customers. Every entry has a problem it solves, a trigger, the systems it touches, the logical flow, the side effects, and the gotchas we hit along the way.

It is **not** a workflow export. There is no runnable code, no `.json` dump, no credentials. The patterns are described platform-agnostically so you can implement them in n8n, Rewst, Power Automate, Make, Zapier, or hand-rolled code — whatever your MSP already uses.

If you've ever wondered "what's the actual list of automations a mature MSP has running?" — that's what this is.

## Who this is for

- **MSP owners and technical leaders** building an automation roadmap.
- **MSP automation engineers** looking for prior art on a specific integration.
- **Service-delivery managers** trying to figure out what to take off their team's plate next.
- **Vendor partners** (PSA, RMM, security stack) curious how their platform gets orchestrated in production.

## How to read this repo

Three entry points:

1. **By category** — start at [`patterns/README.md`](./patterns/README.md). Ten categories, each with its own folder. Skim category descriptions, click into the patterns that match your problems.
2. **By MSP tool** — start at [`taxonomy/tools.md`](./taxonomy/tools.md). Pick your stack (ConnectWise PSA, NinjaOne, Microsoft Graph, etc.), see every pattern that touches it.
3. **By maturity** — start at **`docs/maturity-model.md`** *(coming)*. A suggested sequence: foundations → hygiene → recon → cost → identity → reporting → customer-tenant.

If you're brand new to MSP automation, the maturity model is the right starting point. If you already automate and want ideas, browse by category or tool.

## Pattern card schema

Every entry follows the same schema (see [`docs/pattern-card-template.md`](./docs/pattern-card-template.md)):

| Field | What it means |
|---|---|
| **Category** | One of the 10 directories under `patterns/`. |
| **Primary tools** | The MSP tools the pattern reads from or writes to. |
| **Orchestrator** | Where it runs — usually labeled `tool-agnostic` so you can pick your platform. |
| **Status** | `Production`, `In review`, or `Pattern only (not yet built)`. |
| **Effort to build** | Rough sizing — `S` (< 1 week), `M` (1–3 weeks), `L` (1+ month). |
| **Problem** | What goes wrong without this. |
| **Trigger** | Cron, webhook, ticket event, form, etc. |
| **Data sources** | Systems read, with the specific objects. |
| **Flow shape** | 5–12 logical steps. No node-level detail. |
| **Outputs / side effects** | What the workflow produces or changes. |
| **Outcome / value** | Why an MSP cares. Quantified where defensible. |
| **Gotchas** | Things that bit us. |
| **Dependencies** | Other patterns this assumes are in place. |

## Categories at a glance

| Category | What's in it | Folder |
|---|---|---|
| PSA / Ticketing | Stale-ticket digests, agreement matchers, auto-close, ticket portal access, callback filtering, recurring-issue detection, contract-expiration ticket creation, organization auto-assignment. | [`patterns/psa-ticketing/`](./patterns/psa-ticketing/) |
| Cross-platform reconciliation | RMM ↔ EDR ↔ documentation tool agent recon, organization/site mapping across stacks, CIPP ↔ CSP recon, offboarded-client RECON across the full stack. | [`patterns/reconciliation/`](./patterns/reconciliation/) |
| Identity, access, onboarding | Self-service password reset (with hybrid detection), RBAC requests with Conditional Access, GDAP customer-tenant authentication, employee onboarding/offboarding. | [`patterns/identity-access/`](./patterns/identity-access/) |
| Security & alerting | Huntress alert enrichment with sign-in logs, S1 quarantine alerts, S1 default-site endpoint cleanup, RMM recurring-alert dedup. | [`patterns/security-alerting/`](./patterns/security-alerting/) |
| License & cost optimization | M365 unused-license detection, Pax8 ↔ M365 ↔ PSA license recon, NCE renewal automation, inactive-product feedback loops. | [`patterns/license-cost/`](./patterns/license-cost/) |
| Azure governance | RI utilization, unattached disks/snapshots, proactive Azure tickets, AVD nightly rebuilds, secure Azure auth, backup monitoring. | [`patterns/azure-governance/`](./patterns/azure-governance/) |
| Patch & lifecycle | OS-patch retry, software-patch retry, Windows 11 upgrade campaigns, EOL/EOS tagging. | [`patterns/patch-lifecycle/`](./patterns/patch-lifecycle/) |
| Reporting | Timesheet HTML reports, password-reset audit reports, IT-managed-services ticket reports, OS patch compliance reports. | [`patterns/reporting/`](./patterns/reporting/) |
| Finance & billing hygiene | Non-billable time conversion, credit-hold release, inactive-additions detection, billing reconciliation, contract expiration alerts. | [`patterns/finance-billing/`](./patterns/finance-billing/) |
| Platform & operations | n8n on-prem setup, secure secrets handling, logging/observability, error-handling pattern. | [`patterns/platform-ops/`](./patterns/platform-ops/) |

## Repo status

This is **v0.1** — a soft launch with one or two flagship patterns per category to demonstrate the schema and breadth. New patterns ship as they're sanitized from the production backlog. See [`ProjectSpec.md`](./ProjectSpec.md) §9 for the roadmap.

## What this repo is *not*

To save you from going looking — these are deliberately out of scope:

- **No workflow exports.** No n8n JSON, no Rewst exports, no Power Automate package files.
- **No client identifiers.** No customer names, no employee names, no ticket numbers, no internal URLs. Anywhere. Ever. See [`Rules.md`](./Rules.md).
- **No tutorials.** Patterns assume working familiarity with the named platforms.
- **No internal policy contents.** We mention that policies exist for production-readiness, change control, security, and AI usage. The text of those policies stays internal.

## Use these patterns

You're encouraged to. The license is **MIT**. Take the patterns, implement them in your own stack, ship them to your customers. Attribution is appreciated but not required.

## Contributing

If you've built a pattern not yet here — or a better version of one that's here — we'll happily review a PR. See [`Rules.md`](./Rules.md) for the contribution rules and the sanitization checklist (which is non-negotiable; this is a public repo).

If you find that something here inadvertently leaks an identifier, please don't open a public issue — email the maintainer (see contact below) and we'll fix it.

## Talk to us

If you want to:

- talk through one of these patterns,
- get help building a roadmap like this for your MSP,
- compare notes on what's working and what isn't, or
- just nerd out about MSP automation —

reach out.

> **Contact:** [info@xentek.ca](mailto:info@xentek.ca) · [xentek.ca](https://xentek.ca)

We don't have a customer-acquisition pitch baked into this repo. The bet is simple: if you read this and it's useful, you'll know where to find us.

## License

[MIT](./LICENSE)

---

*Maintained by Xentek. The work catalogued here was built by a working MSP automation team over multiple years. Names, identifiers, and internal references have been removed; the patterns themselves are real.*
