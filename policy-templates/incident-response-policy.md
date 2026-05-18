# Incident Response Policy

**Owner:** [Owner Role — typically CTO, CISO, or Head of Engineering]
**Effective Date:** [Effective Date]
**Next Review:** [Effective Date + 1 year]
**Version:** 1.0

## Purpose

This policy establishes how [Company Name] detects, responds to, contains, recovers from, and learns from information security incidents. A well-rehearsed incident response process limits damage, satisfies legal and contractual obligations, and produces the evidence needed by customers, auditors, and regulators.

## Scope

This policy applies to all suspected or confirmed information security incidents affecting [Company Name]'s systems, data, or services, including those operated by third parties. It applies to all personnel involved in detection, triage, response, or communication during an incident.

## Policy statements

### 1. Definitions

1.1. **Event** — an observable occurrence (a log entry, an alert, a user report). Most events are not incidents.

1.2. **Incident** — an event, or chain of events, that compromises or threatens to compromise the confidentiality, integrity, or availability of [Company Name] information, systems, or services.

1.3. **Severity** — a classification of the incident's actual or expected impact, used to drive response cadence and escalation. Severities are defined in Section 3.

1.4. **Breach** — a subset of incidents involving confirmed unauthorized acquisition, access, or disclosure of personal or otherwise regulated data. Breach status drives notification obligations.

### 2. Incident response phases

2.1. The incident response process consists of the following phases:
- **Preparation** — ongoing readiness work (tooling, runbooks, training, on-call)
- **Detection and analysis** — recognizing that an event is an incident and characterizing it
- **Containment** — limiting the scope and impact
- **Eradication** — removing the cause and any artifacts the attacker may have left behind
- **Recovery** — returning to normal operations and confirming the environment is clean
- **Post-incident activity** — root cause analysis, lessons learned, and follow-up actions

2.2. Each phase shall be documented in the incident record. The team shall not declare an incident closed until all phases are complete (or explicitly waived with justification).

### 3. Severity classification

3.1. Severities are defined as:
- **SEV1 — Critical** — Active compromise of production data or systems; significant customer impact; suspected or confirmed breach. Engages full response team, leadership notification within one (1) hour.
- **SEV2 — High** — Significant security control failure; partial customer impact; high likelihood of escalation. Response within four (4) hours; leadership notification same business day.
- **SEV3 — Medium** — Control failure with limited impact; no immediate customer effect. Response within one (1) business day.
- **SEV4 — Low** — Minor issue not affecting customers or production. Response within five (5) business days.

3.2. The initial severity is assigned by the incident commander and may be raised or lowered as facts develop. Severity changes shall be recorded in the incident record.

### 4. Roles during an incident

4.1. **Incident Commander (IC)** — runs the response. Designated from a roster of trained engineers; rotates with the on-call schedule for SEV1/SEV2.

4.2. **Subject Matter Experts (SMEs)** — engineers responsible for the affected system(s); join at the IC's request.

4.3. **Communications Lead** — coordinates internal and external communication during SEV1/SEV2 events. May be the IC for smaller incidents.

4.4. **Executive Sponsor** — leadership representative kept informed and consulted on decisions with company-level impact.

4.5. **Legal / Privacy Counsel** — engaged for any incident with potential breach implications, regulatory exposure, or media interest.

### 5. Detection

5.1. Incidents may be detected through:
- Security monitoring tools (SIEM, EDR, cloud-native detection services)
- Application or infrastructure logs and alerts
- Vulnerability scans and bug bounty reports
- Customer reports
- Reports from employees through [Reporting Channel — e.g., #security or security@company.com]
- Third-party notifications (vendor breach notice, government, law enforcement)

5.2. Anyone discovering or suspecting an incident shall report it without delay through [Reporting Channel]. Failure to report a suspected incident is itself a violation of this policy.

5.3. Reports shall be acknowledged within thirty (30) minutes during business hours and within two (2) hours outside business hours.

### 6. Triage and declaration

6.1. The on-call responder (or named recipient) shall triage reports to determine whether an incident is warranted.

6.2. If triage indicates an incident, the responder shall:
- Open an incident record in [Incident Tracking System — e.g., Jira, Linear, PagerDuty incident or a dedicated post-mortem doc]
- Page the appropriate IC for SEV1/SEV2
- Open a dedicated communication channel (e.g., a Slack channel named `inc-yyyy-mm-dd-short-name`)
- Begin maintaining a timeline of actions and decisions

6.3. The incident record shall include, at minimum: identifier, severity, summary, status, start time, IC, affected systems and data, timeline, decisions, and (when closed) root cause and remediation actions.

### 7. Containment, eradication, and recovery

7.1. The IC shall prioritize containment to limit damage before eradication. Containment may include disabling accounts, rotating credentials, blocking traffic, isolating systems, or taking services offline.

7.2. Containment actions shall be logged in the timeline with the actor, time, and rationale.

7.3. Eradication shall remove the cause — patching vulnerabilities, removing malware, terminating attacker access, fixing misconfigurations.

7.4. Recovery shall restore service and confirm the environment is clean before normal operations resume. Recovery shall include validation steps (clean scans, verified backups, monitoring for re-occurrence).

### 8. Communication

8.1. Internal communication shall use the incident channel and IC updates. Decisions involving customers, regulators, or legal action shall be coordinated with the Communications Lead and Legal Counsel.

8.2. **Customer notification** — for incidents involving customer data, notifications shall be issued per contractual terms (often within forty-eight (48) to seventy-two (72) hours of confirmation) and per applicable regulation. The Communications Lead drafts the notification; Legal Counsel reviews.

8.3. **Regulatory notification** — breaches involving personal data may trigger notification to data protection authorities (e.g., within seventy-two (72) hours under GDPR, varying timelines under U.S. state laws). Legal Counsel determines applicability.

8.4. External communication (status page, public statements) shall be coordinated through the Communications Lead. Personnel shall not communicate with media, customers, or third parties about an active incident without authorization.

### 9. Post-incident review

9.1. Every SEV1/SEV2 incident, and any incident at the IC's discretion, shall be followed by a post-incident review (PIR) within fifteen (15) business days of closure.

9.2. The PIR shall identify root cause(s), contributing factors, what worked, what did not, and corrective and preventive actions with owners and due dates.

9.3. PIRs shall be blameless. The focus is on systemic improvement, not individual fault. Findings shall be recorded in the incident record.

9.4. Action items from PIRs shall be tracked to completion in [Action Tracking System].

### 10. Evidence preservation and forensics

10.1. During SEV1/SEV2 incidents involving possible criminal activity or significant breach, evidence shall be preserved per Legal Counsel's guidance — logs retained, affected systems imaged, chain of custody maintained.

10.2. Cloud and application logs relevant to incidents shall be retained per the [Data Retention Policy](data-retention-policy.md).

### 11. Training and exercises

11.1. Personnel in incident response roles shall receive role-specific training annually.

11.2. [Company Name] shall conduct at least one (1) tabletop exercise per year covering a realistic incident scenario. Results shall be documented and used to update runbooks and this policy.

### 12. Metrics

12.1. The [Owner Role] shall report incident metrics to leadership at least quarterly, including count by severity, mean time to detect (MTTD), mean time to respond (MTTR), and status of PIR action items.

## Roles and responsibilities

| Role | Responsibility |
|---|---|
| [Owner Role] | Maintain this policy; oversee program; report to leadership |
| Incident Commander | Lead response during an incident |
| SMEs | Provide technical depth and execute remediation |
| Communications Lead | Coordinate internal/external messaging |
| Legal Counsel | Advise on breach status, notifications, evidence |
| All personnel | Report suspected incidents promptly |

## Review and approval

This policy shall be reviewed at least annually by the [Owner Role] and approved by leadership. The policy shall also be reviewed after any SEV1 incident or significant tabletop finding.

## Related SOC 2 criteria

- **CC7.2**
- **CC7.3**
- **CC7.4**
- **CC7.5**

Definitions are available in the AICPA Trust Services Criteria document.

## Revision history

| Version | Date | Author | Summary of changes |
|---|---|---|---|
| 1.0 | [Effective Date] | [Owner Role] | Initial version |

---

Generated and maintained by SimpleAudit — https://simpleaudit.io
