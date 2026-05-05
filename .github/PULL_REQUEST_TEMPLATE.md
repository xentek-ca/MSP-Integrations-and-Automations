<!--
Thanks for contributing to this catalogue. Please fill out the sections below.
This template embeds the sanitization checklist from Rules.md §4 — every PR is
reviewed against it.
-->

## What this PR changes

<!-- One paragraph. Be specific. "Adds the X pattern" / "Updates the Y card with new gotchas" / "Fixes broken cross-link" -->

## Type of change

<!-- Check all that apply -->

- [ ] New pattern card
- [ ] Update to an existing pattern card
- [ ] New anti-pattern card
- [ ] New ADR
- [ ] Taxonomy / index update
- [ ] Editorial / prose tightening
- [ ] Documentation (README, template, guidance)
- [ ] Other (describe)

## Sanitization checklist

**Required.** Run through every item before requesting review.

- [ ] No client / customer names anywhere — body, headings, file names, image alt text, links.
- [ ] No employee names. Roles only.
- [ ] No internal URLs (`app.rewst.io/organizations/...`, `*.app.n8n.cloud/workflow/...`, internal SharePoint, internal git repos, ConnectWise instance URLs, vendor portal tenant URLs).
- [ ] No ticket numbers, incident numbers, or change numbers.
- [ ] No tenant IDs, subscription IDs, organization UUIDs, or workflow IDs.
- [ ] No API keys, OAuth client IDs/secrets, webhook URLs, or any secret-looking string.
- [ ] No copy-pasted screenshots from internal tools.
- [ ] No code blocks containing real endpoints, real org slugs, or real account identifiers.
- [ ] File names are kebab-case and don't reference clients.
- [ ] Commit messages and PR description do not reference any of the above.

## If this PR adds a pattern card

- [ ] Follows [`docs/pattern-card-template.md`](../docs/pattern-card-template.md).
- [ ] `Status` field set honestly (`Production`, `In review`, `Pattern only`).
- [ ] `Gotchas` section is non-empty.
- [ ] `Dependencies` section is filled in with cross-links to related patterns.
- [ ] Mermaid diagram in the `Flow shape` section.
- [ ] Linked from `patterns/README.md` and `taxonomy/tools.md` and `taxonomy/categories.md` where relevant.

## If this PR adds an anti-pattern or ADR

- [ ] Follows the format established in `docs/anti-patterns/` or `docs/decision-records/`.
- [ ] Linked from the folder's README index.

## Editorial check

- [ ] Active voice, present tense, concrete verbs.
- [ ] No vendor-marketing language (`leverages`, `seamless`, `synergy`, `best-in-class`).
- [ ] Reads like a peer engineer wrote it.
- [ ] No emojis unless intentionally illustrative.

## Anything else reviewers should know?

<!-- e.g. "I'm unsure whether this should be its own pattern or a section of the existing X" -->
