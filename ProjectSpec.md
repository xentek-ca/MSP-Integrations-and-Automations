# MSP Automation Patterns — Project Specification

**Publisher:** Xentek
**Audience:** MSP owners, automation engineers, service-delivery managers, MSP TAMs
**License:** MIT
**Status:** Public — sanitized patterns only

---

## 1. Vision

Most MSPs know automation could pay for itself many times over. Few have a concrete picture of *what* to automate, in *what order*, against *which* tools. This repository fixes that.

It is a public, sanitized catalogue of automation patterns that Xentek has actually built, tested, and run in production across the standard MSP stack — ConnectWise PSA, NinjaOne, SentinelOne, Huntress, Liongard, Auvik, Microsoft 365 / Entra, Azure, Pax8, and adjacent tools. Patterns are described platform-agnostically so they can be implemented in n8n (the default), Rewst, Power Automate, Make, Zapier, or hand-rolled code.

The goal is not to publish runnable workflows. The goal is to compress a multi-year automation roadmap into something an MSP peer can read in an afternoon and use to (a) decide what to build next, (b) avoid the obvious traps, and (c) start a conversation with us if they want help moving faster.

## 2. Goals and non-goals

### Goals
- Make the breadth of MSP automation visible. Show categories, dependencies, and sequencing.
- Give each pattern enough depth — problem, trigger, data sources, flow shape, outcome — that a practitioner can scope it.
- Stay tool-agnostic at the pattern level; tag with concrete MSP tools so searchers find their stack.
- Be a credible artifact. No vapor patterns. Everything in here was built and run.

### Non-goals
- Not a runnable code repository. No exports, no `.json` workflow dumps, no environment configs.
- Not a tutorial. Patterns assume baseline familiarity with the named platforms.
- Not a product pitch. Lead-gen is downstream of being useful.

## 3. Audience and call to action

Primary readers are MSP technical leaders evaluating their own automation roadmap, and automation engineers looking for prior art. Secondary readers are vendor partners (PSA, RMM, security stack) curious about how their platform is being orchestrated.

The repo's call to action is **community plus soft lead-gen**:

> *"Steal these patterns. If you want to talk through one — or want help building a roadmap like this for your MSP — get in touch."*

Contact path lives in the repo `README.md` and points to a Xentek email/landing page (to be set before publish).

## 4. Repository structure

```
/
├── README.md                  # Public-facing front door
├── Rules.md                   # Contribution + sanitization rules
├── ProjectSpec.md             # This file
├── LICENSE                    # MIT
├── patterns/
│   ├── README.md              # Index of patterns by category and by tool
│   ├── psa-ticketing/         # ConnectWise / ticketing automations
│   ├── reconciliation/        # Cross-platform asset/agent recon
│   ├── identity-access/       # IAM, password reset, RBAC, GDAP
│   ├── security-alerting/     # Huntress, S1, sign-in audit, quarantine
│   ├── license-cost/          # M365, Pax8, Azure RI, NCE
│   ├── azure-governance/      # RI, backups, unattached resources, AVD
│   ├── patch-lifecycle/       # Patch retries, EOL/EOS, Win11
│   ├── reporting/             # Timesheets, password resets, ticket reports
│   ├── finance-billing/       # Non-billable conversion, credit hold, recon
│   └── platform-ops/          # Logging, error handling, secure auth, change control
├── taxonomy/
│   ├── tools.md               # Each MSP tool with the patterns that touch it
│   └── categories.md          # Each category with its patterns and pre-reqs
└── docs/
    ├── how-to-use-this-repo.md
    ├── pattern-card-template.md
    └── maturity-model.md      # Suggested order to build patterns in
```

Pattern files are named `kebab-case.md`. One pattern per file.

## 5. Pattern card schema

Every pattern is a Markdown file that follows this schema. The template lives at `docs/pattern-card-template.md` and is enforced by review (see `Rules.md`).

```markdown
# <Pattern Name>

**Category:** <one of the directories under /patterns>
**Primary tools:** <e.g. ConnectWise PSA, NinjaOne, Microsoft Graph>
**Orchestrator:** <n8n | Rewst | tool-agnostic>
**Status:** <Production | In review | Pattern only (not yet built)>
**Effort to build:** <S | M | L>  *(rough: S < 1 week, M 1–3 weeks, L 1+ month)*

## Problem
What goes wrong without this automation. One paragraph. Concrete.

## Trigger
What kicks the workflow off — schedule (cron), webhook, form submission, ticket event, etc.

## Data sources
Bulleted list of systems read from, with the specific objects (tickets, agreements,
devices, users, licenses, alerts).

## Flow shape
A short numbered list — 5–12 steps — describing the logical sequence. No node-level
detail, no platform-specific syntax.

## Outputs / side effects
What the workflow produces: tickets created/updated, emails sent, time entries written,
records changed in third-party systems, dashboards updated.

## Outcome / value
Why an MSP cares. Tie to revenue, leakage, hygiene, security posture, NPS, or hours
saved. Quantitative when defensible (e.g. "eliminates ~X tickets/month of manual recon");
qualitative otherwise.

## Gotchas
Things that bit us. API quirks, rate limits, edge cases, data-quality assumptions.

## Dependencies
Other patterns this one assumes are in place (e.g. organization mapping, secure auth).

## Related patterns
Cross-links.
```

## 6. Taxonomy

### 6.1 Categories (initial set)

| Category | What's in it |
|---|---|
| **PSA / Ticketing** | Stale-ticket digests, agreement matchers (timesheet/ticket/cancelled), auto-close, ticket portal access, callback filtering, recurring-issue detection, contract-expiration ticket creation, organization auto-assignment, error-handling ticket creation, sync-error company remap. |
| **Cross-platform reconciliation** | RMM ↔ EDR ↔ documentation tool agent recon, organization/site mapping across stacks, CIPP ↔ CSP recon, offboarded-client RECON across the full stack, agreement-additions ↔ tooling recon. |
| **Identity, access, onboarding** | Self-service password reset (with hybrid detection), RBAC request workflow with Conditional Access, GDAP customer-tenant authentication, M365 Partner Center auth, employee onboarding, employee offboarding, ticket-portal access form. |
| **Security & alerting** | Huntress alert enrichment with Graph sign-in logs, S1 quarantine alerts, S1 default-site endpoint cleanup, RMM recurring-alert dedup, RMM ↔ EDR delta alerts. |
| **License & cost optimization** | M365 unused-license detection, Pax8 ↔ M365 ↔ PSA license recon, NCE renewal automation, NCE upcoming-license + user mapping, inactive-product feedback loops, agreement renewal notifications. |
| **Azure governance** | RI utilization weekly check, unattached disks/snapshots/public IPs/VMs, proactive Azure tickets, AVD nightly rebuilds, secure Azure auth, backup monitoring, customer-tenant backup recon. |
| **Patch & lifecycle** | OS-patch retry, software-patch retry, Windows 11 upgrade campaigns, EOL/EOS tagging, lifecycle-manager-driven ticket notes. |
| **Reporting** | Timesheet HTML reports (service desk + SE), password-reset audit reports, IT-managed-services ticket reports, proactive ticket compliance reports, OS patch compliance reports, inactive-device email to client POC. |
| **Finance & billing hygiene** | Non-billable time conversion, credit-hold release, inactive-additions detection on active agreements, billing reconciliation, agreement-count recon, contract-expiration alerts, annual-increase notifications. |
| **Platform & operations** | n8n on-prem setup, secure secrets handling, logging/observability, error-handling pattern, production-readiness policy, change-control policy *(policy *existence* documented; contents not published)*. |

### 6.2 Primary MSP tools tagged across patterns

ConnectWise PSA · NinjaOne (NinjaRMM) · SentinelOne · Huntress · Liongard · Auvik · Microsoft 365 (Graph, Entra, Exchange) · Microsoft Azure (ARM, Graph, AVD) · Pax8 · Citrix · Dropsuite · Mimecast · IT Glue · CIPP

`taxonomy/tools.md` is the inverted index — pick a tool, get the patterns that touch it.

## 7. What is *not* in scope for the public repo

The following are deliberately excluded. They are listed here so the exclusion is visible and intentional:

- **Client-specific business-process automations.** Anything that automates a single client's internal business systems (proprietary inventory tools, custom database reads, client-specific finance reports, client-specific routing). These don't generalize as MSP patterns, and even sanitized they can leak the client.
- **Workflow URLs and IDs.** No `app.rewst.io/...` links, no `*.app.n8n.cloud/workflow/...` links, no internal SharePoint links, no internal repository links.
- **Workflow exports.** No `.json`, no node graphs, no credentials, no secrets, no environment-specific config.
- **Internal policy contents.** We acknowledge that policies exist for production-readiness, change control, security standards, AI/automation use, MCP servers, and release cycles — but the policy text stays internal.
- **Names.** No client names, no employee names, no ticket numbers, no support-case identifiers.

## 8. Maturity model

`docs/maturity-model.md` orders the categories into a suggested implementation sequence. The short version:

1. **Foundations first.** Organization/site mapping, secure auth, error-handling pattern, logging. Without these, every later automation is fragile.
2. **Hygiene wins.** PSA agreement matchers, auto-close, sync-error remap. Cheap, visible, reduce service-desk noise.
3. **Recon wins.** RMM ↔ EDR ↔ documentation tool agent recon. Catches the silent "we're not protecting what we think we're protecting" problem and produces tickets the SOC actually wants.
4. **Cost & license wins.** M365 unused-license, Azure RI, Pax8/PSA recon. Often pays for the entire automation program in the first year.
5. **Identity & lifecycle.** Password reset, RBAC, onboarding/offboarding. Highest user-visible value; also highest risk if rushed.
6. **Reporting & finance hygiene.** After the above are stable, automate the reports leadership keeps asking for.
7. **Customer-tenant work.** GDAP, customer-side data access, customer-tenant alerting. Hardest to make safe.

## 9. Roadmap for the repo itself

| Milestone | Contents |
|---|---|
| **v0.1 — Soft launch** | This file, `Rules.md`, `README.md`, `LICENSE`, the pattern card template, and ~15 highest-leverage pattern cards (one per category) plus their taxonomy indices, anti-pattern cards, and architecture decision records. |
| **v0.2** | All ~80 sanitizable patterns from the source backlog, plus `taxonomy/` indices. |
| **v0.3** | `maturity-model.md`, cross-pattern dependencies wired up, "start here" reading paths. |
| **v0.4** | One or two flagship deep-dives (heavily anonymized) with diagrams. |
| **Ongoing** | New patterns as new automations ship to production. |

## 10. Editorial principles

- **Plain prose. No vendor-marketing voice.** If a sentence sounds like a webinar, rewrite it.
- **Specific over generic.** "Reduces stale L2 tickets older than 48 hours" beats "improves ticket hygiene".
- **No screenshots of internal tools.** If a diagram helps, redraw it abstractly.
- **No bragging, no false modesty.** Describe what was built and what it does. Let the breadth speak for itself.
- **Keep cards short.** A reader should be able to scan a card in under two minutes.

## 11. Success metrics

Success looks like:

- Inbound conversations from MSP peers asking "how did you build X?" or "can you help us build this?"
- Citations or links from MSP communities (Reddit r/msp, MSP Geek, peer groups, vendor user groups).
- Time-to-first-conversation: how long after publish before the first inbound reach-out.
- Conversion: of inbound conversations, how many become paid engagements.

Pure GitHub stars are vanity. Conversations are the metric.
