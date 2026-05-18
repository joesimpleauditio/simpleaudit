# Evidence Types — What Auditors Want, Common Gaps, and Cadence

SOC 2 fieldwork is essentially structured evidence review. The auditor asks for samples drawn from a population, and your job is to produce records that show the control operated as designed across the period.

This guide covers the major categories of evidence most SOC 2 Type 2 reports require, with practical notes on the gaps most teams discover late and the cadences that produce the cleanest evidence.

## 1. Access management evidence

### What auditors want

- **Provisioning records** showing that access was requested, approved by an appropriate authority, and granted. They will sample new hires or new access grants from across the period and trace each one.
- **Deprovisioning records** showing access was removed at termination or role change. Same-day revocation for termination is the typical expectation.
- **Access reviews** showing periodic re-validation of who has access to what. Reviews include the reviewer's identity, the scope (system, user list), decisions (retained, modified, removed), and remediation timeline.
- **Privileged access reviews** (often more frequent than general reviews) showing who holds administrative or root-level access and that the population is still justified.

### Common gaps

- **Untracked SaaS access.** Direct-to-app sign-ups outside SSO are invisible to access reviews. Audit will surface the SaaS-by-SaaS reality eventually.
- **Service accounts and API keys missing from reviews.** They are real privileged access, and they need to be reviewed.
- **"Reviewed" without evidence of the decision.** A spreadsheet listing 50 users with no annotations is not a review; it is a roster. Each user's row needs a decision (retained / modified / removed) and, where modified or removed, a remediation note.
- **Terminations completed but not documented.** The actions happened; the checklist was never filled in. The auditor cannot accept "trust me."
- **Quarterly reviews that slipped.** Three reviews in a year instead of four creates a documented gap in coverage.

### Recommended cadence

- **Quarterly** — general user access reviews for production systems, source repositories, customer-data stores.
- **Quarterly** — privileged access reviews (administrators, break-glass accounts).
- **Annually** — SaaS application reviews for tools handling sensitive data but with less frequent change.
- **Per event** — onboarding and offboarding records, with a documented checklist.

## 2. Change management evidence

### What auditors want

- **Code review records** — pull requests with approvals from at least one non-author, linked to a work item where applicable.
- **CI pipeline records** — automated tests, security checks, and quality gates that passed before merge.
- **Deployment records** — who deployed what, when, and to which environment.
- **Approvals for significant changes** — schema migrations, security-control changes, infrastructure changes — with evidence of broader review.
- **Rollback or fix-forward records** for failed changes.

### Common gaps

- **Branch protection not enforced uniformly.** A repo exists where direct-to-main commits are possible; the auditor finds the one merged change that bypassed review.
- **Pipeline overrides** that quietly disabled tests for "urgent" deploys. Each override is a control bypass.
- **"ClickOps" changes in production consoles.** No PR, no record. Auditors find these by comparing infrastructure-as-code to actual cloud state.
- **Manual data fixes** without a ticket or script. Bulk SQL run in a console produces no audit trail.

### Recommended cadence

- **Per change** — every change leaves the contemporaneous trail (PR, pipeline, deploy log).
- **Monthly** — sample a few deploys for self-audit. Confirm the audit trail is reachable and complete.
- **Quarterly** — measure deployment metrics (change failure rate, time to recover) and report.

## 3. Vendor management evidence

### What auditors want

- **Vendor inventory** showing every third party handling customer data or critical services, with data types, risk tier, and review status.
- **Due diligence records** for each in-scope vendor — current SOC 2 report (or equivalent), security questionnaire response, contract terms.
- **Annual review records** — evidence each in-scope vendor was reviewed within the year, with the date, reviewer, and findings.
- **Subprocessor list updates** — when a subprocessor was added or removed, with notification to customers per contract.
- **DPAs and security exhibits** in contracts.

### Common gaps

- **The inventory is missing things.** Marketing's email-sending service, Engineering's error-tracking tool, Sales' enrichment platform — each may have customer data and each may have been signed up by an individual without procurement involvement. The auditor will ask.
- **Vendor SOC 2 reports more than 12 months old.** A SOC 2 Type 2 has a period; the report covers up to a year ago. If you have not received the next one, you cannot demonstrate currency.
- **No record of *reviewing* the SOC 2 report.** Receiving the PDF is not the same as reading it. The auditor will ask what exceptions you noticed and how you responded.
- **Subprocessor changes not communicated.** Contracts typically require advance notice; missing the notice is a customer commitment breach.

### Recommended cadence

- **Per vendor onboarding** — due diligence record.
- **Annually** — refresh of each in-scope vendor (review of latest SOC 2 / questionnaire, contract status, ongoing fit).
- **Quarterly** — sweep for new SaaS subscriptions and bring shadow IT into inventory.
- **As needed** — subprocessor notice when adding or removing a listed vendor.

## 4. Risk management evidence

### What auditors want

- **Risk register** — a living document of identified risks with assessed level, owner, treatment decision, and status.
- **Annual risk assessment** — minutes, attendees, scope, output. Often a workshop or series of interviews.
- **Material risk acceptances** with leadership approval in writing and expiration dates.
- **Linkage** — risk register items mapped to controls. The auditor wants to see that identified risks have corresponding controls or accepted-risk decisions.

### Common gaps

- **Stale register.** Last reviewed 14 months ago, no record of treatment progress on open items. Auditors are unforgiving here because the register is supposed to be living.
- **Risks identified, no owner.** Without an owner, the item drifts.
- **Risk acceptance without expiration.** Indefinite "accept" decisions effectively defeat the process.
- **Identified risks not in the register.** Incidents or near-misses surfaced risks that never made it into the register.

### Recommended cadence

- **Annually** — full assessment cycle with workshops and approval.
- **Quarterly** — review open items, update status, escalate aged items.
- **Per event** — material change triggers risk reassessment.

## 5. Security awareness training evidence

### What auditors want

- **Completion records** for new-hire training (within 30 days) and annual refresher (within 12 months).
- **Role-specific training records** — engineers' secure coding, administrators' operational security.
- **Phishing simulation results** — campaign metrics (reach, click rate, report rate), with evidence of follow-up training for repeat clickers.
- **Content showing the training is current** — the slides or modules reference recent threats and current policies.

### Common gaps

- **The 30-day rule is missed.** A new hire was granted production access on day five and completed training on day forty-five. The auditor will catch this in the new-hire sample.
- **Completion claimed without records.** The training platform was changed mid-period and the prior completion data is lost.
- **Phishing simulations skipped a quarter.** The cadence broke; either reset the policy expectation or fix the cadence.

### Recommended cadence

- **Per new hire** — initial training within 30 days, recorded.
- **Annually** — refresher for all personnel, recorded.
- **Quarterly** — phishing simulation with documented results.

## 6. Incident response evidence

### What auditors want

- **Incident records** for every reported incident: identifier, severity, timeline, decisions, root cause, remediation, lessons learned.
- **Post-incident review documents** for SEV1/SEV2 incidents, with action items tracked to closure.
- **Tabletop exercise records** — date, scenario, participants, observations, action items.
- **Sample of zero incidents during the period** — if you genuinely had no SEV1/SEV2 incidents, the auditor will ask how you know (which means you need monitoring evidence).

### Common gaps

- **"Incidents" handled informally without records.** A team patched a vulnerability after a disclosure but never opened an incident; the auditor finds the patch deployment with no upstream record.
- **Action items from PIRs unclosed.** Either close them or document the decision to defer.
- **Tabletop conducted but not documented.** A productive afternoon with the team turns into "we did one, I think it was March" come audit time.
- **No tabletop at all.** Annual exercise is a common expectation; skipping it is a finding.

### Recommended cadence

- **Per incident** — record opened immediately, updated through the timeline, closed with PIR for SEV1/SEV2.
- **Annually** — at least one tabletop or functional exercise.
- **Quarterly** — leadership review of incident metrics and PIR action items.

## 7. Backup and recovery evidence

### What auditors want

- **Backup schedule** showing what is backed up, how often, retention, where stored.
- **Backup completion logs** for the period — what succeeded, what failed, what was retried.
- **Restoration test records** — at least annually, evidence a restoration was performed end-to-end and the data matched expectations.
- **DR exercise records** — at least annually, evidence of a functional or full-failover test.

### Common gaps

- **Backups configured but never tested.** Many teams discover at the worst possible moment that the backup is incomplete, corrupt, or untested for restoration.
- **DR exercise that did not actually fail over.** A tabletop counts as a DR exercise, but for production-critical systems the auditor will expect functional validation at least once a year.
- **Backup retention shorter than retention policy claims.** A policy saying "backups retained one year" with actual configuration of 30 days is a documented control failure.

### Recommended cadence

- **Continuous** — backup operations themselves.
- **Daily/weekly** — automated completion monitoring with alerts on failure.
- **Annually** — restoration test (end-to-end, documented).
- **Annually** — DR exercise (functional or full failover for critical systems).

## 8. Vulnerability management evidence

### What auditors want

- **Scan output** showing the scanning programs ran on schedule across the period.
- **Findings inventory** — open vulnerabilities by severity, with owner and target remediation date.
- **Remediation records** — fixes deployed, with linkage to the original finding.
- **Penetration test report** within the year, with findings tracked to closure or risk acceptance.
- **Aging metrics** — how long findings sit open by severity, with leadership reporting.

### Common gaps

- **Critical findings aging past SLA.** The auditor compares the policy ("Critical within 15 days") to the data ("Critical finding open 87 days") and writes a finding.
- **Pen test conducted but findings never tracked.** The report PDF sits in a drive; remediation status is unknown.
- **Scanning coverage gaps.** A subset of containers or repositories is excluded from scans without documented justification.

### Recommended cadence

- **Continuous to weekly** — automated scans.
- **Annually** — external penetration test.
- **Monthly** — vulnerability dashboard reviewed by [Owner Role].
- **Quarterly** — leadership reporting on aging metrics and material findings.

---

A program that produces this evidence regularly — without heroic effort in the weeks before the audit — is in good shape. A program where everyone scrambles to assemble artifacts at audit time produces lower-quality evidence and tends to discover process gaps that the auditor then writes up.

The templates in [examples/](examples/) show what consistent, audit-ready evidence looks like in three of the most commonly sampled categories.

Generated and maintained by SimpleAudit — https://simpleaudit.io
