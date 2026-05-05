# GitHub Discussions Setup

GitHub Discussions can't be configured by file — it's enabled in the repository settings. This is the playbook for setting it up correctly the first time.

## 1. Enable Discussions

Repository **Settings** → **General** → **Features** → check **Discussions**.

## 2. Recommended categories

Replace GitHub's default categories with this set:

| Category | Format | Purpose |
|---|---|---|
| **Announcements** | Announcement | Maintainer-only posts. Release notes, new pattern drops, repo updates. |
| **Q&A** | Question / Answer | Visitor questions about applying patterns, adapting them to other orchestrators, or implementation details. Answers can be marked accepted. |
| **Show and tell** | Open-ended discussion | "We implemented your X pattern in Power Automate, here's what we learned." Encourages reciprocal sharing. |
| **Pattern proposals** | Open-ended discussion | Lightweight proposals for new patterns before they become full issue + PR. Lower-friction alternative to the issue template. |
| **Anti-patterns from the field** | Open-ended discussion | Visitors share things they tried that didn't work. Mirrors the `docs/anti-patterns/` folder culturally. |
| **Tooling and orchestrators** | Open-ended discussion | Comparisons, gotchas, vendor-specific notes. The place for "we're considering Rewst vs n8n, what do you think?" threads. |
| **General** | Open-ended discussion | Catch-all for anything that doesn't fit the above. Keep an eye on it; if a topic recurs, promote it to its own category. |

## 3. Pinned discussions

After enabling, pin two discussions:

1. **"Welcome — read this first"** — short post explaining what the repo is, what to expect, where to file what (issues vs discussions vs private email).
2. **"What pattern would you like us to publish next?"** — open thread for visitor input on the v0.2 / v0.3 backlog. This is also a soft signal that the repo is actively maintained.

## 4. Moderation posture

- Respond to every Q&A thread within a few business days even if just to acknowledge. Silence kills Discussions.
- Mark good answers as Accepted. Future readers will land on these via search.
- Convert recurring questions into FAQ entries (planned: `docs/faq.md`) and pattern-card gotchas where relevant.
- Don't let Show-and-tell become a soliloquy — actively reply to community contributions.

## 5. Linkage from elsewhere in the repo

- `README.md` "Talk to us" section should link to Discussions.
- `.github/ISSUE_TEMPLATE/config.yml` should reference Discussions in `contact_links` (already configured).
- Pattern cards' "Related patterns" sections can reference Discussions threads where deeper conversation has happened.

## 6. What to track

Discussion volume is a leading indicator. Specifically watch:

- Time to first response (target: under 2 business days).
- Question / answer ratio in Q&A (more answers than questions = healthy).
- Show-and-tell volume month-over-month.
- Conversion rate from Discussions thread to private contact (the actual lead-gen funnel).

If Discussions becomes a graveyard (questions sit unanswered, threads have no replies), the affordance is *worse* than not having it. Either commit to moderating or close the category — limp Discussions are a negative credibility signal.
