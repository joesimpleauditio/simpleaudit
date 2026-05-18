# SOC 2 Readiness Checklist

A checklist of controls and evidence items that organizations preparing for SOC 2 Type 2 audit should have in place. Grouped by Trust Services Criteria. Each item begins with the owning function in brackets (e.g., `[Engineering]`).

Items are stated as actions: when you can check the box honestly — meaning the control is in place *and* producing evidence — you have addressed the item.

> See [`README.md`](README.md) for guidance on how to use this checklist.

---

## Common Criteria — Security (the foundation of every SOC 2 report)

### CC1 — Control Environment

- [ ] **[Leadership] Code of conduct documented and acknowledged.** Every employee and contractor has acknowledged the company's code of conduct, including ethical expectations and reporting procedures for violations. Acknowledgement records are retained.
- [ ] **[HR] Background checks performed where legally permitted.** Background screening is part of hiring, scoped to the role. Records of completion are retained.
- [ ] **[Leadership] Organizational chart maintained.** A current organizational chart documents reporting lines, key functions, and segregation of duties for security-sensitive roles.
- [ ] **[Leadership] Information security responsibilities assigned.** A named individual (often the CTO or CISO) owns the information security program. The responsibility is documented in a job description, charter, or policy.
- [ ] **[Leadership] Board or leadership oversight of security program.** Leadership receives reports on the security program at least quarterly, and the cadence is documented.
- [ ] **[HR] Job descriptions exist for security-sensitive roles.** Roles handling sensitive data, production access, or compliance responsibilities have documented descriptions including security expectations.
- [ ] **[HR] Performance evaluation process exists.** Personnel are evaluated periodically, and the evaluation includes performance against security responsibilities where applicable.
- [ ] **[HR] Disciplinary process documented.** A process exists to address policy violations consistently, including security and confidentiality violations.

### CC2 — Communication and Information

- [ ] **[Security] Information security policy approved and published.** The top-level policy exists, is approved by leadership, dated, and accessible to all personnel.
- [ ] **[Security] Topic-specific policies approved.** Policies for access control, change management, incident response, vendor management, vulnerability management, data classification, retention, business continuity, security awareness, and acceptable use exist and are approved.
- [ ] **[Security] Policies reviewed at least annually.** Each policy shows evidence of review within the last 12 months. The next review date is documented.
- [ ] **[Security] Policies acknowledged by personnel.** Every employee and contractor acknowledges the relevant policies at hire and at least annually. Acknowledgement records are retained.
- [ ] **[Security] Channel exists for reporting security concerns.** Personnel know where to report concerns (a security email address, internal channel, or anonymous reporting line). The channel is documented in policy and reinforced in training.
- [ ] **[Communications] External communication channel for security disclosures.** A documented, monitored channel (typically `security@[domain]` or a security.txt file) exists for external parties to report vulnerabilities or concerns.
- [ ] **[Security] Customer communication for changes affecting security.** Material changes to subprocessors, security commitments, or service operations are communicated to customers per contractual notice periods.
- [ ] **[Legal] Confidentiality obligations in place.** Employees, contractors, and material third parties have signed appropriate confidentiality agreements (or have signed master service agreements that include confidentiality).

### CC3 — Risk Assessment

- [ ] **[Security] Risk management policy documented.** A policy describes how risks are identified, assessed, treated, monitored, and communicated.
- [ ] **[Security] Risk register exists and is current.** A risk register records identified risks with assessed level, treatment, owner, and status. The register has been reviewed within the last 12 months.
- [ ] **[Security] Annual risk assessment completed.** A formal risk assessment exercise has been conducted within the last 12 months. Output is documented and approved by leadership.
- [ ] **[Security] Risk treatment plans documented for material risks.** Material risks have documented treatment plans with action items, owners, and due dates.
- [ ] **[Leadership] Risk acceptance approved at appropriate level.** Risks accepted above the documented threshold are approved by leadership in writing, with expiration dates.
- [ ] **[Security] Change-driven risk reviews.** Material changes (new product line, major architecture change, M&A, new regulation) trigger documented risk reassessment.
- [ ] **[Security] Fraud risks considered.** The risk assessment explicitly considers fraud scenarios — internal and external.

### CC4 — Monitoring Activities

- [ ] **[Security] Control monitoring activities documented.** The organization has documented which controls are monitored, by whom, and how frequently.
- [ ] **[Security] Internal audit or internal assessment performed.** At least once per year, an internal review of selected controls is performed, with findings tracked to remediation. (For small companies, this may be conducted by the security owner with peer review; the auditor cares about the discipline, not the title.)
- [ ] **[Security] Findings tracking system maintained.** Findings from internal reviews, audits, penetration tests, and security incidents are tracked centrally to closure. Aged findings are escalated.
- [ ] **[Security] Quarterly review of program effectiveness.** Leadership reviews key risk indicators, incident metrics, and audit results at least quarterly.

### CC5 — Control Activities

- [ ] **[Security] Control library maintained.** The organization maintains a list of its key controls, mapped to Trust Services Criteria.
- [ ] **[Security] Segregation of duties documented.** For critical functions (production deployment, financial transactions, access provisioning) responsibilities are split or compensating controls are documented.
- [ ] **[Security] Manual controls have documented procedures.** Where controls rely on manual execution (access reviews, vendor reviews, training tracking), procedures and templates exist.
- [ ] **[Security] Automation used where practical.** Repetitive controls (account deprovisioning, access logging, vulnerability scanning) are automated where reasonable.

### CC6 — Logical and Physical Access Controls

- [ ] **[IT] Identity provider used as system of record.** Personnel identities are managed in a corporate identity provider (Okta, Microsoft Entra ID, Google Workspace, JumpCloud). Local accounts on production systems are limited to documented exceptions.
- [ ] **[IT] MFA enforced for all corporate accounts.** Multi-factor authentication is enforced for sign-in to the identity provider and for all administrative access.
- [ ] **[IT] MFA enforced for production access.** All access to production systems, infrastructure, and code repositories requires MFA.
- [ ] **[Engineering] Production console access uses SSO.** Cloud provider consoles use SSO from the corporate identity provider, not standalone accounts.
- [ ] **[Engineering] No standing administrative access on critical systems.** Where feasible, administrative privileges are granted just-in-time with logged justification, not held continuously.
- [ ] **[IT] Access request process documented and ticketed.** New access requires a ticket, an approval, and a fulfillment record. Self-service grants without an audit trail are not permitted for sensitive systems.
- [ ] **[IT] Least-privilege grants documented.** New access is scoped to the minimum needed. Periodic reviews confirm scopes have not expanded.
- [ ] **[IT] Annual access reviews documented.** User access to production systems, source code, and customer data is reviewed at least annually (quarterly preferred). The reviewer, scope, decisions, and remediation timelines are recorded.
- [ ] **[IT] Privileged access reviews documented.** Administrative and privileged accounts are reviewed at least quarterly.
- [ ] **[HR / IT] Onboarding access checklist used.** New hires receive access through a documented checklist tied to their role. Records are retained.
- [ ] **[HR / IT] Termination access removed same business day.** Access for departing personnel is revoked on the same day as termination (or immediately for involuntary departures). A termination checklist documents revocation.
- [ ] **[IT] Inactive accounts disabled.** Inactive accounts (e.g., 90+ days without sign-in) are flagged and disabled per policy.
- [ ] **[IT] Service accounts inventoried.** Service accounts, API keys, and other non-interactive credentials are inventoried with owners, purposes, and rotation schedules.
- [ ] **[Security] Secrets stored in approved secrets manager.** Production secrets (database credentials, API keys, signing keys) are stored in a secrets manager — not in code, config files, or chat. Access is logged.
- [ ] **[Security] Encryption at rest on production data stores.** All databases, object storage, and file storage containing customer data are encrypted at rest with provider-managed or customer-managed keys.
- [ ] **[Security] Encryption in transit for customer data.** All connections handling customer data use TLS 1.2 or higher. Weak ciphers are disabled.
- [ ] **[Engineering] Key management documented.** Encryption keys are managed in a KMS with documented rotation, access controls, and recovery procedures.
- [ ] **[Engineering] Production administrative interfaces are not internet-exposed.** Bastion hosts, jump boxes, or zero-trust access are used; direct internet exposure of administrative interfaces is prohibited.
- [ ] **[Engineering] Network segmentation enforced.** Production environments are logically separated from development and staging. Access boundaries are documented.
- [ ] **[Engineering] Public ingress documented.** Public-facing endpoints are inventoried with their purpose, owner, and security configuration (TLS, WAF, rate limiting).
- [ ] **[Office Ops] Physical access controlled (if office exists).** Office access uses badges or controlled entry; visitors sign in; sensitive areas are restricted. (For fully remote organizations, this control may be marked not applicable with documented justification.)
- [ ] **[Engineering] Endpoint protection on company devices.** Company-issued laptops have managed endpoint security, full-disk encryption, screen lock, and patching enabled. Device inventory is current.
- [ ] **[Engineering] Mobile device management in place where BYOD or company-issued mobile is used.** Mobile devices accessing company email or applications meet documented baseline requirements.

### CC7 — System Operations

- [ ] **[Engineering] Centralized logging in place.** Production application logs, authentication logs, and audit logs flow to a central platform with retention aligned to policy.
- [ ] **[Security] Log retention meets policy.** Security-relevant logs are retained per the data retention policy (typically 1–2 years minimum).
- [ ] **[Security] Security monitoring covers production.** Automated detection (cloud-native or SIEM-based) generates alerts on anomalous activity. Alert rules are documented and tuned.
- [ ] **[Security] On-call rotation for security alerts.** Security alerts route to an on-call rotation with documented response time targets. After-hours response is tested.
- [ ] **[Security] Vulnerability scans run on schedule.** Application dependency, container, and cloud configuration scans run on the cadence defined in the vulnerability policy.
- [ ] **[Security] External penetration test performed annually.** An independent qualified party has tested in-scope systems within the last 12 months. Findings have been triaged and remediated or accepted with documentation.
- [ ] **[Security] Vulnerability remediation tracked to SLA.** Open vulnerabilities have owners and target dates aligned to severity. Aged findings are reported to leadership.
- [ ] **[Security] Incident response policy approved.** An incident response policy exists, is approved, and is communicated to relevant personnel.
- [ ] **[Security] Incident response tabletop exercise within last 12 months.** A tabletop or functional exercise has been performed within the last 12 months, with results documented.
- [ ] **[Security] Incident records preserved.** Past incidents have records including timeline, severity, root cause, remediation, and lessons learned. Records are retained per policy.
- [ ] **[Engineering] Health and availability monitoring in place.** Production services are monitored for availability and performance; on-call is alerted on relevant signals.
- [ ] **[Engineering] Capacity monitoring in place.** Resource utilization is monitored with thresholds that trigger review before exhaustion.

### CC8 — Change Management

- [ ] **[Engineering] Code review required.** Every change merged to a production branch is reviewed by a non-author. Self-approval is prohibited. Branch protection enforces the rule.
- [ ] **[Engineering] CI pipeline enforces gates.** Every change passes automated tests, security checks, and required quality gates before merge.
- [ ] **[Engineering] Deployment is automated and logged.** Production deployments run through pipelines that record who, what, and when. Manual deploys, where required, are documented.
- [ ] **[Engineering] Infrastructure as code used for production.** Production infrastructure is defined in code (Terraform, Bicep, CloudFormation, etc.), reviewed, and version-controlled. Manual cloud changes are documented and converted to IaC.
- [ ] **[Engineering] Schema changes reviewed for compatibility.** Database migrations are reviewed for backwards compatibility, with rollback plans for destructive changes.
- [ ] **[Engineering] Separation of duties on production deploys.** The same individual does not unilaterally author, approve, and deploy without compensating automated controls (peer review, pipeline approvals).
- [ ] **[Engineering] Production secrets rotated on a schedule.** Credentials with documented rotation requirements are rotated per policy.
- [ ] **[Engineering] Test environments mirror production architecture.** Where feasible, staging or pre-production mirrors production for meaningful pre-deploy validation.
- [ ] **[Engineering] Feature flags or similar safeguards for risky changes.** Significant changes deploy behind feature flags or staged rollouts when feasible, allowing controlled exposure and fast rollback.

### CC9 — Risk Mitigation (including business continuity)

- [ ] **[Security] Business impact analysis (BIA) documented.** Critical business processes are identified with their dependencies, recovery time objectives (RTOs), and recovery point objectives (RPOs).
- [ ] **[Engineering] Backups in place for production data.** Production data is backed up on a schedule consistent with RPO. Backups are encrypted and stored separately from primary data.
- [ ] **[Engineering] Backup restoration tested annually.** Backup integrity is verified at least annually through a documented restoration test.
- [ ] **[Engineering] DR exercise performed annually.** At least one disaster recovery exercise is performed each year for critical systems, with documented results.
- [ ] **[Security] Vendor management policy approved.** A policy describes how third parties are selected, monitored, and offboarded.
- [ ] **[Security] Vendor inventory maintained.** A vendor inventory records all third parties processing customer data or providing critical services, with risk tier and review status.
- [ ] **[Security] Subprocessor list maintained.** Subprocessors handling customer data are listed publicly and updated as the list changes, per contractual notice obligations.
- [ ] **[Security] Subservice SOC 2 reports received annually.** Subservice organizations (e.g., cloud providers, identity providers) provide SOC 2 or equivalent attestations within the last 12 months, reviewed for relevant exceptions.
- [ ] **[Security] Insurance in place where applicable.** Cyber-insurance, professional liability, and other insurance products appropriate to the business are maintained, with documentation accessible.

### Awareness, training, and personnel

- [ ] **[HR] New-hire security training within 30 days.** Every new hire completes security awareness training within 30 days of start. Records are retained.
- [ ] **[HR / Security] Annual security awareness refresher.** All personnel complete refresher training annually. Completion is tracked centrally.
- [ ] **[Security] Engineer-specific secure coding training annual.** Engineers complete secure coding training (in-depth, role-specific) annually.
- [ ] **[Security] Phishing simulations conducted quarterly.** Simulated phishing campaigns run at least quarterly. Click rates and report rates are tracked.

### Asset management

- [ ] **[IT] Hardware asset inventory current.** Company-issued devices are inventoried with assigned user, hostname, and lifecycle status. Returns at termination are tracked.
- [ ] **[IT] Software inventory maintained.** Approved software and SaaS subscriptions are inventoried with owners and renewal dates.

---

## Availability (A)

Applies if the SOC 2 report scope includes Availability. SaaS providers usually add this category.

- [ ] **[Engineering] Service level commitments documented.** Customer-facing SLAs (uptime targets, response times) are documented in contracts or the service description, and the production architecture is designed to meet them.
- [ ] **[Engineering] Multi-zone or multi-region production architecture.** Critical production systems run across multiple availability zones (or regions, where the RTO requires).
- [ ] **[Engineering] Auto-scaling or capacity headroom maintained.** Capacity is monitored and scaled in advance of saturation. Capacity reviews occur on a documented cadence.
- [ ] **[Engineering] Backup retention aligned to RPO.** Production data backups are retained per the BIA and DR plans.
- [ ] **[Engineering] Backup restoration tested.** At least annually, restoration is validated end-to-end in a non-production environment.
- [ ] **[Engineering] DR exercise conducted within 12 months.** Functional or full-failover exercises are documented, with RTO/RPO measured.
- [ ] **[Engineering] Environmental controls for owned facilities.** For any company-owned infrastructure facilities, environmental controls (power, cooling, fire suppression) are documented. (Pure-cloud organizations may mark this not applicable, deferring to the cloud provider's SOC 2.)
- [ ] **[Engineering] Status page maintained.** A public or customer-accessible status page communicates current service health and historical incidents.
- [ ] **[Engineering] Incident communications process for outages.** Major outages trigger customer communications per documented procedure.
- [ ] **[Engineering] Capacity planning reviewed quarterly.** Capacity trends, growth rates, and saturation risks are reviewed at least quarterly.
- [ ] **[Engineering] Disaster recovery plans documented per critical system.** Each critical system has a runbook covering failover, restoration, and validation.

---

## Confidentiality (C)

Applies if the SOC 2 report scope includes Confidentiality. Often added by B2B SaaS providers handling proprietary customer data.

- [ ] **[Security] Data classification policy documented.** A policy defines classification levels and handling requirements per level.
- [ ] **[Security] Customer data classified Confidential or higher by default.** Customer data is handled per its classification, with cross-team awareness.
- [ ] **[Engineering] Encryption at rest for Confidential data.** All Confidential and Restricted data is encrypted at rest in production systems.
- [ ] **[Engineering] Encryption in transit for Confidential data.** TLS 1.2+ for all transmission of Confidential and Restricted data, including internal traffic where feasible.
- [ ] **[Engineering] Access to Confidential data restricted.** Production access to Confidential data is limited to need-to-know with logged audit trails.
- [ ] **[Security] Data sharing with third parties governed by DPA.** Confidential data shared with third parties is covered by data-processing agreements.
- [ ] **[Engineering] Secure data disposal procedures.** End-of-life or end-of-contract data is disposed of using methods preventing recovery; disposal is recorded.
- [ ] **[Legal] Customer commitments around confidentiality documented.** Contractual confidentiality commitments are recorded and traceable to specific controls.
- [ ] **[Security] Bulk export of Confidential data is controlled and logged.** Export operations from production data stores require justification and produce audit records.
- [ ] **[Security] Data leakage prevention controls in place where applicable.** Where DLP tooling is in scope, deployment, tuning, and exception handling are documented.

---

## Processing Integrity (PI)

Applies if the SOC 2 report scope includes Processing Integrity. Common where the product's value depends on accuracy of computed results (financial systems, billing, analytics platforms processing customer data on customer's behalf).

- [ ] **[Engineering] Input validation in place.** Customer inputs are validated against schema; invalid inputs are rejected or quarantined with documented behavior.
- [ ] **[Engineering] Authentication and authorization for processing requests.** Every request that triggers processing is authenticated and authorized.
- [ ] **[Engineering] Processing completeness checks.** Batch and stream processing record counts and reconcile inputs to outputs; failed records are surfaced.
- [ ] **[Engineering] Processing accuracy validated.** Key calculations are covered by automated tests; representative samples are spot-checked.
- [ ] **[Engineering] Error handling produces audit trails.** Processing errors are logged with sufficient detail to investigate and remediate.
- [ ] **[Engineering] Retries and idempotency.** Long-running operations are idempotent so retries do not produce duplicate effects.
- [ ] **[Engineering] Output validated before delivery.** Outputs to customers or downstream systems are validated against expected schema and integrity rules.
- [ ] **[Engineering] Reconciliation cadence for material flows.** Material data flows (billing, financial settlements) are reconciled on a documented cadence.
- [ ] **[Engineering] Manual interventions logged.** Manual data corrections in production are scripted, approved, executed by authorized personnel, and recorded.
- [ ] **[Engineering] Change controls for processing logic.** Changes affecting processing logic are reviewed by a domain expert, not only by general code reviewers.

---

## Privacy (P)

Applies if the SOC 2 report scope includes Privacy. Required when the product handles material personal data subject to privacy commitments.

- [ ] **[Legal] Privacy notice published.** A privacy notice describes what personal data is collected, why, how long it is retained, and the rights individuals have.
- [ ] **[Legal] Personal data categories inventoried.** A data map describes categories of personal data, where it is stored, who processes it, and the legal basis or contractual purpose.
- [ ] **[Legal] Consent mechanism documented where required.** Where applicable law requires consent, the mechanism is documented with evidence of capture.
- [ ] **[Legal] Data subject rights handling process documented.** Procedures exist for access, rectification, deletion, restriction, and portability requests, with timelines aligned to applicable law (e.g., 30 days under GDPR; varies by U.S. state law).
- [ ] **[Legal] Privacy DPIA process documented.** A data protection impact assessment process exists for new processing activities involving sensitive personal data.
- [ ] **[Security] Personal data retention enforced.** Personal data is deleted when its retention period ends, per the data retention policy.
- [ ] **[Security] Cross-border transfer mechanism in place where applicable.** Where personal data is transferred across regulated borders (e.g., EU to U.S.), a documented transfer mechanism (SCCs, adequacy decisions, transfer assessments) is in place.
- [ ] **[Security] Pseudonymization or minimization applied where practical.** Personal data is minimized in production where feasible and pseudonymized in non-production environments.
- [ ] **[Engineering] Audit trails for access to personal data.** Access to identifiable personal data in production is logged and reviewable.
- [ ] **[Legal] Vendor DPAs in place for personal-data processors.** Every vendor that processes personal data has signed a data-processing agreement with appropriate clauses.

---

## Final checks before fieldwork

- [ ] **[Security] Walk every control with critical eyes.** Walk through each policy line by line. Where does practice not match policy? Where is the evidence thin? Resolve before the audit, not during.
- [ ] **[Security] Confirm system description draft.** The SOC 2 system description (the narrative of your services, infrastructure, and controls) is drafted and reviewed.
- [ ] **[Security] Confirm scope and additional categories with auditor.** The audit scope (which Trust Services Categories, which systems, which observation period) is agreed in writing with the auditor before the period begins.
- [ ] **[Security] Confirm CUEC list with auditor.** Complementary user entity controls — what your *customers* are responsible for — are drafted and reviewed with the auditor.
- [ ] **[Security] Confirm sampling expectations.** Discuss sampling approaches with the auditor (period, sample sizes) so evidence collection aligns with what will be tested.
- [ ] **[Operations] Evidence requests have a single front door.** Audit evidence requests come through a single channel with a single owner who tracks status, deadlines, and responsiveness.
- [ ] **[Operations] Audit log of audit interactions.** Maintain a record of all questions asked, evidence provided, and decisions made during fieldwork. This becomes invaluable for the next audit cycle.

---

When every applicable box can be checked honestly, you are ready for fieldwork. Anything you cannot check is a known gap — fix it, accept it with documented compensating controls, or carve it out of scope.

Generated and maintained by SimpleAudit — https://simpleaudit.io
