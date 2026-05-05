# Anti-Patterns

A pattern catalogue is one half of the picture. The other half — arguably the more useful half — is the things that *didn't* work. Failures are how a practitioner learns; documenting them is how the next practitioner avoids the same lesson.

Every card in this folder describes something we tried, why it didn't work, and what we did instead. The format is short and uniform:

- **Pattern category** — which type of work this affects.
- **What we tried** — the approach in a sentence or two.
- **Why it didn't work** — the specific failure mode, not a vague disappointment.
- **What we did instead** — the resolution, with a link to the resulting pattern when one exists.
- **Lesson** — the generalizable takeaway for someone in a similar spot.

The cards are deliberately short. Failure post-mortems get long; anti-pattern cards just need to describe the trap and the way out.

---

## Index

| ID | Title | Category |
|---|---|---|
| [AP-0001](./ap-0001-competing-with-vendor-native-automation.md) | Don't compete with vendor-native automation | platform-ops |
| [AP-0002](./ap-0002-misleading-workflow-time-metrics.md) | Workflow-level time-saved metrics are misleading | platform-ops |
| [AP-0003](./ap-0003-bulk-customer-tenant-ops-without-csp-recon.md) | Don't bulk-operate on customer tenants without CSP/PSA recon | identity-access |
| [AP-0004](./ap-0004-execution-data-on-auth-flows.md) | Don't persist execution data on auth-touching workflows | platform-ops |
| [AP-0005](./ap-0005-retroactive-error-handling.md) | Retroactive error handling is more expensive than building it from day one | platform-ops |
| [AP-0006](./ap-0006-vendor-api-drift.md) | Vendor APIs drift silently — instrument every integration with a heartbeat | platform-ops |
| [AP-0007](./ap-0007-not-all-sku-mapping-is-worth-automating.md) | Not all SKU/identifier mapping is worth automating | license-cost |
