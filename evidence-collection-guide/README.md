# Evidence Collection Guide

Concrete guidance for the artifacts SOC 2 auditors actually sample, why they sample them, and how to produce them without grinding your engineering team to a halt.

## What's in this section

- **[evidence-types.md](evidence-types.md)** — A category-by-category breakdown of the evidence types auditors look for, common gaps, and recommended cadences.
- **[examples/access-review-template.md](examples/access-review-template.md)** — A realistic template for documenting a quarterly user access review.
- **[examples/vendor-risk-assessment-template.md](examples/vendor-risk-assessment-template.md)** — A vendor security review template you can adapt per-vendor.
- **[examples/incident-response-postmortem-template.md](examples/incident-response-postmortem-template.md)** — A post-incident review template that meets audit expectations for incident records.

## Why evidence is harder than it looks

The hardest part of SOC 2 is rarely *implementing* the controls. It is producing the *evidence* that the controls operated effectively across the entire observation period.

A common pattern: a team implements MFA on day one of the observation window, then forgets to keep evidence of *who is enforcing the rule and how*. Six months later, when the auditor asks "show me the MFA enforcement was active on a sample of days," the team has to reconstruct evidence after the fact — and the auditor may or may not accept that.

Strong evidence has three properties:

1. **Contemporaneous** — generated as the control operated, not reconstructed later.
2. **Attributable** — clear about who performed the activity and when.
3. **Complete** — covers the scope claimed by the control.

The most effective programs treat evidence as a *byproduct* of normal operations, not a separate audit task. A ticketing system that records access requests and approvals produces evidence automatically; a Slack DM does not.

## Sequencing your evidence work

If you are months away from the observation window:

1. **Identify which controls produce evidence automatically** (logs, audit trails, ticketing system records, CI pipeline outputs). These are your foundation.
2. **Identify which controls require deliberate recordkeeping** (access reviews, vendor reviews, tabletop exercises, training tracking). Establish templates and cadences now.
3. **Set calendar reminders for recurring cadences.** A quarterly access review that you forget for one quarter creates a gap the auditor will find.
4. **Start producing evidence early.** Several months of records before the window opens is better than the same number of records produced inside the window — partly because it proves the cadence is stable, partly because you discover gaps in your process while there is still time to fix them.

If you are already in the observation window:

1. **Make sure all current evidence is captured and named consistently.** Lost evidence is the most common audit problem.
2. **For controls you know are operating, but where evidence is thin, add scaffolding immediately.** A documented access review that runs from this point forward is better than no review at all, even if the auditor's sample includes earlier months when the review did not run.
3. **For controls you cannot satisfy by the end of the window**, work with your auditor on remediation. SOC 2 reports include exceptions and management's response; a known exception with a documented remediation plan is much better than a discovered exception you cannot explain.

## A note on evidence formatting

Auditors are accustomed to a wide range of formats — screenshots, PDFs, exported CSVs, ticket links, signed documents. What matters is that the evidence is *identifiable* (clearly tied to the control), *complete* (covers the scope), and *legible* (a third party can read it).

The templates in the [examples/](examples/) directory are deliberately simple Markdown. You can adapt them to your team's preferred tooling — a Confluence page, a Notion doc, a Word file, a Google Doc, a structured ticket. The structure is what the auditor cares about, not the format.

---

Generated and maintained by SimpleAudit — https://simpleaudit.io
