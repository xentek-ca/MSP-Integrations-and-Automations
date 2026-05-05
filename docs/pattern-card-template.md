# Pattern Card Template

Copy this file when adding a new pattern. Replace each `<…>` placeholder. Keep all sections — if a section truly does not apply, write `*N/A*` and a one-line reason.

A pattern card should be readable in under two minutes. If yours runs longer than ~250 lines, it's probably trying to be a tutorial. Trim it back to the pattern.

---

```markdown
# <Pattern Name>

**Category:** <one of: psa-ticketing, reconciliation, identity-access, security-alerting, license-cost, azure-governance, patch-lifecycle, reporting, finance-billing, platform-ops>
**Primary tools:** <e.g. ConnectWise PSA, NinjaOne, Microsoft Graph>
**Orchestrator:** <n8n | Rewst | tool-agnostic>
**Status:** <Production | In review | Pattern only (not yet built)>
**Effort to build:** <S | M | L>

## Problem
<One paragraph. Concrete. Answer "what goes wrong without this automation?"
Avoid generic phrasing. Tie to a real operational pain — leakage, hygiene, latency,
risk, cost, customer satisfaction.>

## Trigger
<One line. Schedule (with cadence), webhook, ticket event, form submission, etc.>

## Data sources
<Bulleted list of systems read from. For each, name the specific objects.>
- <System>: <objects, e.g. "tickets created or closed in last 8h on configured boards;
  most recent update; current status">
- <System>: <objects>

## Flow shape
<5–12 numbered steps. Logical, not node-level. A reader should be able to translate
this to their own orchestrator without seeing your implementation.>
1. <Step>
2. <Step>
3. <Step>

## Outputs / side effects
<What the workflow produces or changes. Be specific: tickets created, time entries
written, records changed in third-party systems, emails sent, dashboards updated,
files written.>

## Outcome / value
<Why an MSP cares. Tie to revenue, leakage, hygiene, security posture, NPS, or hours
saved. Quantify when you can defend the number; qualitative is fine when you can't.
Avoid marketing voice — describe what changed for the team or the customer.>

## Gotchas
<Things that bit us. Real lessons, not platitudes. Every production automation has
gotchas; if yours has none, you haven't run it long enough yet.>
- <Gotcha>
- <Gotcha>

## Dependencies
<Patterns this one assumes are in place — by file path under /patterns. Also list
any non-pattern prerequisites (e.g. "ConnectWise API credentials with read on
Service Tickets and write on Time Entries").>
- [Pattern name](../some-category/some-pattern.md)
- <Non-pattern prerequisite>

## Related patterns
<Cross-links to patterns a reader of this one is likely to also want.>
- [Pattern name](../some-category/some-pattern.md)
```

---

## Section-by-section guidance

### Title
A noun phrase that names the pattern. Problem-oriented beats implementation-oriented:
- Good: `Stale Ticket Daily Digest`, `Unused M365 License Detection`
- Bad: `Daily 9am Cron That Pulls Tickets`, `n8n flow for licenses`

### Category
Must match an existing folder under `patterns/`. If your pattern doesn't fit any of the ten, do not invent one in the same PR — open an issue first proposing the new category.

### Primary tools
Use the canonical product name. *NinjaOne*, not *Ninja*. *Microsoft Entra*, not *Azure AD*. *ConnectWise PSA*, not *ConnectWise Manage*. List 2–4 tools max; if a pattern touches more than four, the *core* tools belong here and the rest belong in `Data sources`.

### Orchestrator
Three valid values:
- `n8n` — when the pattern relies on something n8n-specific (e.g. a particular code-node trick).
- `Rewst` — same, when the pattern is Rewst-specific.
- `tool-agnostic` — the default. Most patterns can be built in any orchestrator and should be labeled this way.

If the production version was built in n8n but the pattern itself is portable, label it `tool-agnostic` and add a parenthetical note: `tool-agnostic (originally n8n)`.

### Status
- `Production` — running in the wild, paying back its build cost.
- `In review` — built and being validated; not yet trusted.
- `Pattern only (not yet built)` — documented but not implemented. Use sparingly; the repo's credibility comes from "we ran this".

### Effort to build
A rough sizing for a competent automation engineer who already knows the platforms involved:
- `S` — under a week.
- `M` — one to three weeks.
- `L` — a month or more.

This is for the *first* implementation. Reusing components across patterns shortens later builds.

### Problem
The most-skipped section, and the most important. If a reader can't tell from the problem paragraph why this automation exists, the rest of the card is wasted.

Concrete is better than general:
- Better: "Service-delivery leadership wants near-real-time awareness of ticket activity on specific PSA boards without checking the PSA UI throughout the day."
- Worse: "Improves ticket visibility for the team."

### Trigger
One line. Be specific about cadence — `Cron — every 8 hours, aligned to UTC` is better than `Scheduled`.

### Data sources
List every system read from, with the specific objects. A reader scanning this section should be able to scope the API permissions they'd need.

### Flow shape
The hardest section to keep tight. Discipline rules:
- Logical steps, not node-level steps. "Query PSA for tickets in last 8h" is logical; "HTTP Request → Set → IF → Loop Over Items" is node-level.
- 5–12 steps. If you need more, you're either documenting at the wrong level or the pattern is actually two patterns.
- Don't include error-handling-as-content here unless it's genuinely part of the pattern. The `error-handling` platform-ops pattern covers the generic version.

### Outputs / side effects
"What changed in the world after the workflow ran?" Be honest about side effects on third-party systems — a reader needs to know whether this pattern writes back to ConnectWise, sends external emails, touches Entra, etc.

### Outcome / value
This section is for the MSP business case. If you've measured impact, say so. If not, describe the operational change in concrete terms (e.g. "the service desk no longer has to manually triage these"). Avoid superlatives.

### Gotchas
Mandatory. Every real automation has gotchas. If you're drafting a pattern card and can't list any, ask the engineer who built it — they'll have at least three. Common categories:
- API quirks (pagination, rate limits, undocumented behavior)
- Time-zone bugs
- Edge cases the prod data exposed
- Data-quality assumptions that turned out wrong
- Permissions/scope issues

### Dependencies
Two flavors:
- **Pattern dependencies** — link to other patterns in this repo by relative path. These build on each other; foundations like `organization-mapping` and `error-handling` are commonly depended on.
- **Non-pattern prerequisites** — credentials, agreed-upon configuration, manual steps performed once before the automation can run.

### Related patterns
Suggestive, not strict. Things a reader of this card is plausibly also interested in.

---

## A note on length

A clean pattern card is 60–150 lines of Markdown. If yours is shorter than 60, it's probably under-described — at minimum, problem, trigger, data sources, flow shape, outcomes, and gotchas should each be present. If it's longer than 250, it's probably either two patterns or trying to be a tutorial; split it or trim it.

## A note on tone

Read [`Rules.md`](../Rules.md) §7 before submitting. Two key reminders:
- Active voice, present tense, concrete verbs.
- No vendor-marketing language (`leverages`, `seamless`, `synergy`, `best-in-class`).
- A pattern card should sound like a peer engineer wrote it for another peer engineer.
