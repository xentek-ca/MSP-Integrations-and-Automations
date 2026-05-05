# Rules — Contribution and Sanitization

This repository is public. Everything in it is read by competitors, customers, vendors, and prospects. These rules exist so contributors — human or AI — keep the repo useful without leaking anything we shouldn't.

If you're not sure whether something belongs in the repo, the default answer is **don't publish it, ask first.**

---

## 1. The cardinal rule

> **No client identifiers. Ever.**

Not company names. Not employee names. Not ticket numbers. Not workflow URLs. Not internal repo URLs. Not screenshots that show any of the above. Not file paths on internal servers. Not Microsoft tenant IDs, Azure subscription IDs, or any UUID that could be googled to a customer.

This rule applies whether the data is in body text, code blocks, image alt text, file names, or commit messages.

---

## 2. What can be published

A pattern card is publishable when **all** of the following are true:

1. It describes a generalizable MSP automation pattern, not a single-client business process.
2. It can be written without any client name, employee name, or internal identifier.
3. It does not include workflow exports, node-level configuration, credentials, secrets, or environment-specific config.
4. The orchestrator-specific details (n8n node IDs, Rewst step IDs) have been replaced with logical descriptions.
5. The pattern was either built to production, or is explicitly labeled `Status: Pattern only (not yet built)`.

If any of those is false, the pattern stays internal.

---

## 3. What must never be published

The following are non-negotiable. A PR that includes any of these gets rejected on sight.

### 3.1 Identifiers
- Client / managed-service customer names, abbreviations, or recognizable nicknames.
- Employee names (current or former), email addresses, phone numbers.
- Ticket numbers, change numbers, incident numbers.
- Microsoft 365 tenant IDs, Azure subscription IDs, Azure tenant domains.
- Customer-tenant or partner-center org UUIDs.

### 3.2 URLs
- Anything under `app.rewst.io/organizations/...`
- Anything under `*.app.n8n.cloud/workflow/...` or any other tenanted automation-platform host
- Internal SharePoint, OneDrive, or wiki URLs
- Internal GitHub / GitLab / Bitbucket repository URLs
- Internal Jira / Confluence / monday.com / Asana URLs
- ConnectWise instance URLs
- Vendor portal URLs that reveal a tenant slug

### 3.3 Artifacts
- Workflow JSON exports (n8n, Rewst, Power Automate, Make, Zapier).
- Code that contains hard-coded org IDs, tenant IDs, API keys, OAuth secrets, or webhook URLs.
- Database schemas of internal systems.
- Internal architecture diagrams that name internal hostnames.

### 3.4 Policies
- Internal policy contents (release cycle, change control, security, AI usage, MCP server policy).
- The fact that we have these policies is fine to mention; the text isn't.

### 3.5 Client-internal automations
- Anything automating a single client's proprietary business application (their inventory system, their finance database, their custom routing logic, their internal email follow-up process). These are not MSP patterns.

---

## 4. Sanitization checklist (use before every PR)

Run through this list before opening a PR. The reviewer will run it again.

- [ ] No client/customer names anywhere — body, headings, file names, image alt text, links.
- [ ] No employee names. Replace with role: "the technical account manager", "the service-desk lead", "the responsible engineer".
- [ ] No internal URLs. If a link is needed, link to vendor public docs or remove the link.
- [ ] No ticket numbers, incident numbers, or change numbers.
- [ ] No tenant IDs, subscription IDs, organization UUIDs, or workflow IDs.
- [ ] No API keys, OAuth client IDs/secrets, webhook URLs, or any secret-looking string.
- [ ] No copy-pasted screenshots from internal tools. If a visual is required, redraw it abstractly with an SVG/diagram tool.
- [ ] No code blocks that include real endpoints, real org slugs, or real account identifiers.
- [ ] File names are kebab-case and don't reference clients (e.g. `m365-unused-license-detection.md`, never `acme-corp-licenses.md`).
- [ ] Commit message and PR description do not reference any of the above.
- [ ] Pattern card follows the schema in `docs/pattern-card-template.md`.
- [ ] Status field is set honestly: `Production`, `In review`, or `Pattern only (not yet built)`.

---

## 5. Naming conventions

- **Files:** `kebab-case.md`. Descriptive. Do not start with a tool name unless the pattern is genuinely tool-specific (most aren't).
  - Good: `unused-m365-license-detection.md`, `psa-agreement-matcher.md`
  - Bad: `acme-license-thing.md`, `n8n-flow-42.md`
- **Pattern titles:** Short, descriptive, problem-oriented. "Stale ticket digest" not "Daily 9am Cron Job that Pulls Tickets".
- **Tool names in body text:** Use the canonical product name on first mention (e.g. *NinjaOne* not *Ninja*; *Microsoft Entra* not *Azure AD*; *ConnectWise PSA* not *ConnectWise Manage*). Subsequent mentions can use the short form.
- **Categories:** Use the categories defined in `ProjectSpec.md`. Don't invent new ones in a single PR — propose a category change in a separate issue first.

---

## 6. The "fictional MSP" rewrite test

A useful sanitization test: read the pattern as if it were written about a fictional MSP — call them *Acme MSP* — serving fictional clients *Globex*, *Initech*, and *Stark Industries*. If your pattern still makes sense and is still valuable to the reader after that mental substitution, it's safe. If specific details disappear or stop making sense, those details were carrying client information you didn't realize you were exposing.

---

## 7. Pattern card review checklist

Reviewers run this on every pattern PR.

**Content quality**
- [ ] Problem statement is concrete and answers "what goes wrong without this?"
- [ ] Trigger is specified (schedule, webhook, event, form).
- [ ] Data sources are listed with the specific objects being read.
- [ ] Flow shape is 5–12 numbered logical steps. No node-level detail.
- [ ] Outputs / side effects are listed.
- [ ] Outcome / value is concrete and ideally quantified.
- [ ] Gotchas section exists and is non-empty (every real automation has gotchas).
- [ ] Dependencies on other patterns are linked.

**Sanitization**
- [ ] All items in the §4 checklist verified.

**Style**
- [ ] Reads like a peer engineer wrote it, not a marketing page.
- [ ] No vendor-marketing language ("seamless", "synergy", "leverage", "best-in-class").
- [ ] No emojis unless intentionally illustrative.
- [ ] Active voice. Present tense for what the workflow *does*; past tense for what it *replaced*.

---

## 8. License

This repository is published under the **MIT License**. Contributors agree their contributions are licensed the same way.

Patterns in this repo describe automation approaches. Implementing them in your own MSP is encouraged. If you re-publish or adapt content with attribution, link back to this repo.

---

## 9. Security disclosure

If you find that something in this repository inadvertently leaks a client identifier, internal URL, or other sensitive content, **do not open a public issue.** Email the repository maintainer (contact in `README.md`) with the path and the leak. We will sanitize and rewrite the affected commit history if necessary.

If you find a security vulnerability in any pattern (e.g. a flow shape that has an obvious privilege-escalation gap), please disclose privately the same way.

---

## 10. AI assistant rules

If you are an AI assistant (Claude, Cursor, Copilot Chat, anything similar) editing this repository, the rules in this file apply to you the same as to a human contributor. A short reminder:

- The cardinal rule in §1 is non-negotiable for AI edits as well.
- When in doubt, ask the human collaborator before publishing.
- Never invent client identifiers to fill in a "missing" detail. A pattern card with fewer specifics is fine; a pattern card with fabricated specifics is not.
- Never copy content verbatim from internal sources without running the §4 checklist.

---

## 11. Updating these rules

These rules are living. If a contributor finds a category of leak the checklist doesn't cover, open an issue proposing a checklist item. PRs to `Rules.md` follow the same review process as pattern PRs.
