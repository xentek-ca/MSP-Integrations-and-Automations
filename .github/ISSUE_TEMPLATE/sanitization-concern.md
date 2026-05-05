---
name: Sanitization concern
about: Report content in this repo that may inadvertently leak a client identifier or other sensitive data
title: "[Sanitization] "
labels: sanitization
assignees: ''
---

> **Stop.** If the leak is identifying enough that opening a public issue would *spread* the leak, **do not file this issue.** Email the maintainer privately instead — see the contact path in the repository README. We will scrub and (if necessary) rewrite history.
>
> Use this template only for borderline cases where the content is plausibly safe but you want a second opinion.

## Path / URL of the content in question

<!-- e.g. patterns/psa-ticketing/some-pattern.md, line 24 -->

## What might be a leak

<!--
Be specific but not over-quote. "The phrase X on line N looks like it might be
a real customer name" is enough — don't paste the full block of text.
-->

## What kind of identifier you're worried about

- [ ] Client / customer name
- [ ] Employee name
- [ ] Internal URL (Rewst, n8n cloud, SharePoint, internal git, vendor portal)
- [ ] Ticket / incident / change number
- [ ] Tenant ID / subscription ID / org UUID
- [ ] API key, secret, token
- [ ] Other (describe)

## How obvious is it

- [ ] Clearly identifying — please scrub immediately
- [ ] Plausibly identifying — needs a second look
- [ ] Probably fine but worth flagging
