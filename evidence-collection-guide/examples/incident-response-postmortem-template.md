# Post-Incident Review — [Incident Identifier and Short Title]

**Incident ID:** [INC-YYYY-NNN]
**Short title:** [e.g., "Customer data export accessible to wrong tenant after permissions migration"]
**Severity:** [SEV1 | SEV2 | SEV3 | SEV4]
**Start time:** [YYYY-MM-DD HH:MM TZ — first observable event]
**Detection time:** [YYYY-MM-DD HH:MM TZ — when team became aware]
**Mitigation time:** [YYYY-MM-DD HH:MM TZ — when customer impact stopped]
**Resolution time:** [YYYY-MM-DD HH:MM TZ — when root cause was removed]
**Customer impact:** [Brief — number of customers affected, what they experienced, duration]
**Incident Commander:** [Name]
**PIR facilitator:** [Name — often a different person than the IC]
**PIR date:** [YYYY-MM-DD — within 15 business days of resolution per policy]
**PIR participants:** [List of attendees]

---

## 1. Summary

[Two or three paragraphs. What happened, what caused it, how it was resolved, what the impact was. Should be readable by someone who was not on the response and gives them an accurate picture of events.]

## 2. Timeline

A factual sequence of observable events. All times in [Time Zone]. Source references in brackets where useful (`[log]`, `[Slack #inc-...]`, `[ticket]`).

| Time | Event |
|---|---|
| YYYY-MM-DD HH:MM | [The triggering condition began — e.g., "Deployment of release 2.34.0 to production-us-east-1 completed."] |
| YYYY-MM-DD HH:MM | [First impact event — e.g., "Customer A reported unexpected records in their export."] |
| YYYY-MM-DD HH:MM | [Detection — e.g., "On-call engineer paged via PagerDuty alert XYZ."] |
| YYYY-MM-DD HH:MM | [IC declared SEV2. Incident channel `#inc-2026-09-15-export-perms` opened.] |
| YYYY-MM-DD HH:MM | [Containment action — e.g., "Disabled export endpoint for affected customer tier."] |
| YYYY-MM-DD HH:MM | [Root cause hypothesized — e.g., "Engineer identified tenant-id check missing on export path after permissions migration."] |
| YYYY-MM-DD HH:MM | [Fix deployed.] |
| YYYY-MM-DD HH:MM | [Customers notified per policy.] |
| YYYY-MM-DD HH:MM | [Incident closed.] |

## 3. Root cause analysis

### 3.1 Proximate cause

[What technically went wrong. The "what."]

### 3.2 Contributing factors

[The "why." Use 5-whys, fishbone, or your preferred technique. Aim for systemic factors — process, tooling, knowledge, organizational — not individuals.]

- [Factor 1 — e.g., "Permissions migration PR was merged with reviewer approval but without integration test coverage on the export endpoint."]
- [Factor 2 — e.g., "Existing tests covered access via the standard query path but did not cover the export-specific path that bypassed the shared authorization helper."]
- [Factor 3 — e.g., "Manual QA pass had test coverage for the standard path but did not include export — exports are seldom exercised in QA."]
- [Factor 4 — e.g., "Observability did not include a metric or alert for cross-tenant data access at the row level."]

### 3.3 What we knew vs. what we missed

| What we knew | What we missed |
|---|---|
| [Existing knowledge that informed the system] | [Knowledge that would have prevented or shortened the incident] |
| [...] | [...] |

## 4. Impact assessment

### 4.1 Customer impact

[Quantified where possible. Number of customers affected, types of operations affected, duration. Whether data was actually exposed (vs. just exposable). What customers experienced firsthand.]

### 4.2 Data impact

[Did this incident involve unauthorized access to data? If so:]

- Categories of data potentially affected: [list]
- Number of records potentially affected: [count]
- Confirmed exposure events: [count and details]
- Notification obligations: [analysis — contractual, regulatory]
- Notifications sent: [yes/no, when, to whom, by which channel]

### 4.3 Operational impact

[Internal effort: hours engaged, teams involved, downstream effects on planned work.]

### 4.4 Reputation and trust impact

[Customer-facing communications, status page entries, social/media discussion if any.]

## 5. What went well

[Honest list. The team's strengths during response. Worth recording so the strengths are reinforced.]

- [Detection happened within X minutes; the alert was tuned correctly.]
- [The IC stayed focused on containment before debugging.]
- [Customer communication template was ready; first customer notice went out within Y hours of confirmation.]
- [...]

## 6. What went poorly

[Honest list. The gaps and frictions during response. Not "what people did wrong" — what the system made hard.]

- [The export endpoint was not in our default monitoring; we relied on a customer report for detection.]
- [Roles outside the engineering response team were unclear; the Communications Lead joined 45 minutes in.]
- [Our internal documentation of the permissions migration did not flag the export path as separately affected.]
- [...]

## 7. Action items

Concrete, owned, dated. Every action item links to a tracked ticket.

| # | Action | Why | Owner | Target | Status | Ticket |
|---|---|---|---|---|---|---|
| 1 | Add integration tests for export endpoint covering tenant isolation | Prevent same class of regression | [Owner] | [Date] | Open | [Ticket link] |
| 2 | Add row-level cross-tenant access alert in observability | Catch this class of issue from telemetry rather than customer reports | [Owner] | [Date] | Open | [Ticket link] |
| 3 | Update incident runbook with a "data exposure assessment" step | Reduce time to assess notification obligations | [Owner] | [Date] | Open | [Ticket link] |
| 4 | Document export-related touchpoints in the permissions-migration writeup | Improve handoff for future migrations | [Owner] | [Date] | Open | [Ticket link] |
| 5 | Tabletop scenario informed by this incident | Reinforce playbook in low-stakes setting | [Owner] | [Date] | Open | [Ticket link] |

## 8. Action item follow-up

Action item status will be reviewed at [cadence — e.g., the next quarterly security leadership meeting] until all items are closed.

| Review date | Reviewer | Outcome (open/closed counts; any escalations) |
|---|---|---|
| [YYYY-MM-DD] | [Name] | [Notes] |

## 9. Lessons applied beyond this incident

[Where appropriate, lessons from this incident should be applied to related systems or processes. Note specifically what was generalized.]

- [Example: "The cross-tenant alert from action item #2 was deployed to all data-access endpoints, not just exports."]
- [Example: "Future schema/permissions migrations now require a written touchpoint inventory."]

## 10. Blameless review affirmation

This review was conducted under [Company Name]'s blameless post-incident review practice. The focus was on systems and processes, not individuals. Personnel who took response actions did so in good faith with the information available at the time.

## 11. Distribution and retention

This report has been shared with [audience — e.g., engineering leadership, security team, customer-facing teams as appropriate]. Customer-facing communications and regulatory notifications, if any, are linked in [section/folder].

Storage location: `[Path or link]`

Retention: at least five (5) years per the Incident Response Policy.

---

*Template generated by SimpleAudit — https://simpleaudit.io. Use this template for SEV1 and SEV2 incidents; for lower-severity incidents, sections may be condensed but the core (timeline, root cause, action items) should remain.*
