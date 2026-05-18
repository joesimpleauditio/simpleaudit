# Change Management Policy

**Owner:** [Owner Role — typically CTO or Head of Engineering]
**Effective Date:** [Effective Date]
**Next Review:** [Effective Date + 1 year]
**Version:** 1.0

## Purpose

This policy establishes how changes to [Company Name]'s production systems are proposed, reviewed, approved, deployed, and recorded. The objective is to reduce risk introduced by change — outages, vulnerabilities, regressions, data loss — while preserving the speed of delivery that the business needs.

## Scope

This policy applies to all changes to production systems, including application code, infrastructure (cloud configurations, containers, networks), data structures (database schemas), security controls, and integrations. It applies to all personnel who propose, review, or execute changes. Routine emergency response actions (incident-driven changes) are governed by the [Incident Response Policy](incident-response-policy.md) but the record-keeping requirements of this policy still apply post-incident.

## Policy statements

### 1. Definitions

1.1. **Standard change** — a routine, low-risk, repeatable change executed through approved automation (for example, a code merge that passes the standard pipeline). Standard changes follow the default review and deployment workflow.

1.2. **Significant change** — a change with elevated risk: schema migrations, infrastructure changes, security control changes, large refactors, changes to data-handling code paths, third-party integrations. Significant changes require additional review.

1.3. **Emergency change** — a change required to resolve an incident or otherwise mitigate immediate harm; deployed outside normal review cadence with retrospective documentation.

### 2. Source of truth and versioning

2.1. All production code shall be maintained in [Source Control — e.g., GitHub, GitLab, Azure DevOps] with controlled branching.

2.2. Infrastructure shall be managed as code where practical (Terraform, Bicep, CloudFormation, Pulumi, Kubernetes manifests) and stored in source control.

2.3. Direct production console or CLI changes ("ClickOps") shall be avoided. When unavoidable, changes shall be documented in a change record with the rationale and converted to infrastructure-as-code at the next opportunity.

### 3. Code review

3.1. Every change merged to a production branch shall be reviewed by at least one engineer who did not author the change.

3.2. Self-approval is prohibited. The author may not be the sole reviewer.

3.3. Reviewers shall consider, at minimum: correctness, test coverage, security implications, performance impact, observability, and rollback ability.

3.4. Significant changes shall be reviewed by at least one reviewer with relevant subject-matter expertise.

3.5. Code review records (the pull request, comments, approvals, the time of merge) shall be preserved in source control for at least two (2) years.

### 4. Testing

4.1. Every change shall have appropriate automated tests. New code paths require new tests, except where explicitly justified in the change description.

4.2. The continuous integration pipeline shall run unit, integration, and security tests on every change. Merges shall not proceed if the required pipeline checks fail.

4.3. Significant changes shall be tested in a non-production environment before promotion to production where feasible.

### 5. Approval and authorization

5.1. **Standard changes** are approved when the code review approval(s) and pipeline checks are satisfied.

5.2. **Significant changes** require a documented review by [Change Review Forum — e.g., engineering leadership, an architecture review board, the on-call lead]. The review confirms risk has been considered, rollback is feasible, and necessary stakeholders are aware.

5.3. **Emergency changes** require approval from the on-duty incident commander or designated leadership representative. Approval and rationale are recorded in the incident record. Post-incident, the change is documented and reviewed against this policy.

### 6. Deployment

6.1. Deployments shall be executed through automated pipelines wherever possible. The pipeline records who initiated the deployment, what was deployed, when, and the outcome.

6.2. Deployments to production shall be reversible. Rollback procedures shall be documented and tested for significant changes.

6.3. Manual deployment steps shall be documented in a runbook, executed by an authorized engineer, and recorded.

6.4. Significant changes shall include a deployment plan covering: pre-deployment checks, deployment steps, validation, monitoring window, rollback criteria.

6.5. Where customer-impacting downtime is required, scheduled maintenance windows shall be communicated to customers in advance per contractual notice periods.

### 7. Segregation of duties

7.1. The same individual shall not unilaterally author, approve, and deploy a production change without compensating controls. Compensating controls include: independent code review, automated pipeline approvals, peer pairing during deployment.

7.2. Production credentials and deploy permissions are scoped per [Access Control Policy](access-control-policy.md). Production administrators are a limited group.

### 8. Schema and data changes

8.1. Database schema changes (migrations) shall be reviewed for backwards compatibility, rollback feasibility, and data integrity.

8.2. Destructive schema changes (column drops, table drops, data deletions) shall require explicit approval beyond the standard reviewer and shall be deployed with safeguards (feature flags, staged rollouts, snapshots).

8.3. Bulk data modifications outside the normal application flow (data fixes, backfills, migrations) shall be scripted, reviewed, executed by an authorized engineer, and documented in a change record.

### 9. Configuration changes

9.1. Production configuration changes are within scope of this policy. Toggles, environment variables, feature flags affecting security, availability, or processing integrity shall be treated as significant changes.

### 10. Third-party and library changes

10.1. Third-party library and dependency upgrades shall be reviewed for security advisories, license compatibility, and behavior changes. Major version upgrades are typically significant changes.

10.2. New third-party dependencies shall be evaluated against criteria including: maintenance status, security history, license, and necessity.

### 11. Audit trail

11.1. Each change shall produce a traceable record consisting of, at minimum:
- Source-control commit history
- Pull request with approvals
- Pipeline execution record
- Deployment record (who, what, when)
- Linked work item or ticket (issue, story, incident)

11.2. Audit trails shall be retained for at least two (2) years and made available for sampling during audits.

### 12. Post-change validation

12.1. After deployment, the change author or designated owner shall validate that the change behaves as expected and that monitoring shows no regressions.

12.2. Regressions detected post-deployment shall be handled per the [Incident Response Policy](incident-response-policy.md) and trigger a rollback or fix-forward depending on severity.

### 13. Metrics

13.1. The [Owner Role] shall track and report metrics including: change failure rate, mean time to recover from failed changes, lead time for changes, deployment frequency. Trends shall be reviewed quarterly.

## Roles and responsibilities

| Role | Responsibility |
|---|---|
| [Owner Role] | Maintain this policy; oversee program; track metrics |
| Change authors | Propose changes, write tests, request reviews, validate post-deployment |
| Reviewers | Evaluate changes; consider risk; approve or request modifications |
| Change Review Forum (for significant changes) | Coordinate cross-team awareness |
| Engineering leadership | Resource the program; resolve disputes |
| All personnel | Follow the policy; do not bypass controls |

## Review and approval

This policy shall be reviewed at least annually by the [Owner Role] and approved by leadership. Material changes require leadership approval.

## Related SOC 2 criteria

- **CC8.1**

Definitions are available in the AICPA Trust Services Criteria document.

## Revision history

| Version | Date | Author | Summary of changes |
|---|---|---|---|
| 1.0 | [Effective Date] | [Owner Role] | Initial version |

---

Generated and maintained by SimpleAudit — https://simpleaudit.io
