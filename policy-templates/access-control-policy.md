# Access Control Policy

**Owner:** [Owner Role — typically CTO, CISO, or Head of Engineering]
**Effective Date:** [Effective Date]
**Next Review:** [Effective Date + 1 year]
**Version:** 1.0

## Purpose

This policy defines how access to [Company Name]'s information systems, applications, and data is granted, managed, reviewed, and revoked. It exists to ensure that access is limited to what individuals legitimately need to perform their duties, reducing the risk of unauthorized disclosure, modification, or destruction of information.

## Scope

This policy applies to all systems, applications, infrastructure, and data sources owned, operated, or used by [Company Name], including production environments, internal corporate systems, third-party SaaS applications, and any storage of customer data. It covers all employees, contractors, and third parties who access these resources.

## Policy statements

### 1. Identity management

1.1. Every user shall have a unique, attributable identity. Shared accounts are prohibited except where technically required (for example, certain service accounts), and where prohibited the use shall be documented, approved, and logged.

1.2. Identities for employees and contractors shall be provisioned from [Identity Provider — e.g., Okta, Microsoft Entra ID, Google Workspace] as the system of record. Local accounts on production systems are prohibited except for break-glass purposes.

1.3. User identities shall be tied to the human resource record. Termination or role change in the HR system shall trigger downstream identity changes.

### 2. Access provisioning

2.1. Access to any system, application, or data resource shall be requested through [Ticketing System — e.g., Jira, Linear, ServiceNow] and approved by the resource owner or the requestor's manager (or both, where appropriate).

2.2. Access shall be granted on the principle of **least privilege**: only the minimum permissions necessary to perform the assigned role.

2.3. Privileged or administrative access (root, admin, owner-equivalent roles in production) requires additional approval from the [Owner Role] or a designated alternate.

2.4. Provisioning records — request, approval, fulfillment — shall be retained for at least one (1) year and made available for audit sampling.

### 3. Authentication

3.1. All users authenticating to [Company Name] systems shall use credentials issued by the corporate identity provider. Password-only authentication to production systems is prohibited.

3.2. **Multi-factor authentication (MFA)** is required for:
- All employee and contractor access to [Identity Provider]
- All access to production environments, source code repositories, and customer data
- All privileged or administrative access
- All access to email and file-storage systems containing classified data

3.3. Acceptable MFA factors are FIDO2/WebAuthn (preferred), TOTP authenticator apps, and push notifications. SMS-based MFA shall be disabled unless no alternative exists, and shall not be used for privileged access.

3.4. Where SSO is supported by a SaaS application, SSO shall be configured. Direct-to-application credentials are permitted only when SSO is not technically available and the absence is documented in [Vendor Management Policy] inventory.

### 4. Password and credential management

4.1. Passwords shall meet the following minimum requirements: at least 12 characters, no maximum length below 64 characters, no required composition rules (mixed case, special characters), and screening against published lists of breached passwords.

4.2. Passwords shall not expire on a fixed schedule unless required by contract or regulation. Passwords shall be changed promptly on suspected or confirmed compromise.

4.3. Service account credentials, API keys, and other non-interactive secrets shall be stored in [Secrets Manager — e.g., AWS Secrets Manager, HashiCorp Vault] and rotated at least annually or upon suspected compromise.

4.4. Credentials shall never be embedded in source code, configuration files committed to repositories, chat messages, or email.

### 5. Privileged access

5.1. Standing administrative access shall be minimized. Where possible, privileged actions shall be performed via just-in-time elevation with logged justification.

5.2. Break-glass accounts (root, root-equivalent emergency accounts) shall be:
- Documented in the access inventory
- Secured with MFA and credentials stored in [Secrets Manager]
- Monitored for use with alerting to the [Owner Role]
- Reviewed at least quarterly

5.3. Use of any break-glass account shall be reviewed within one (1) business day, and the justification documented.

### 6. Access reviews

6.1. **User access reviews** shall be performed at least quarterly for production systems, source code repositories, and systems holding customer data. The reviewer is the system owner or designated alternate.

6.2. **Administrative / privileged access reviews** shall be performed at least quarterly.

6.3. **Application-level reviews** for SaaS tools handling sensitive data shall be performed at least annually.

6.4. Review records shall include the reviewer, the date, the scope (users/roles examined), the outcomes (retained, modified, removed), and the remediation timeline for any required changes. Records shall be retained for at least two (2) years.

### 7. Access termination

7.1. Access for terminated employees and contractors shall be revoked on the same business day as termination, or immediately upon involuntary termination.

7.2. Access changes following role transitions shall be completed within five (5) business days.

7.3. The termination workflow shall include: disabling identity-provider account, revoking SSH keys and personal access tokens, recovering company-issued devices, and removing access from systems that do not federate to the identity provider.

7.4. A termination checklist shall be retained per individual for at least two (2) years.

### 8. Remote and third-party access

8.1. Remote access to production systems shall use the corporate VPN, identity-aware proxy, or zero-trust access platform configured by [Company Name]. Direct exposure of administrative interfaces to the public internet is prohibited.

8.2. Third-party access (vendors, contractors, auditors) shall be time-bounded, scoped to the minimum necessary resources, logged, and reviewed at least quarterly.

### 9. Monitoring and logging

9.1. Authentication events, privileged actions, and access changes shall be logged centrally and retained per the [Data Retention Policy](data-retention-policy.md).

9.2. Anomalous access patterns (impossible travel, brute-force attempts, unusual privilege escalation) shall trigger alerts to the [Owner Role] or security team.

## Roles and responsibilities

| Role | Responsibility |
|---|---|
| [Owner Role] | Maintain this policy, oversee access program, approve privileged-access exceptions |
| System / data owners | Approve access requests, perform quarterly reviews |
| Managers | Initiate access changes for direct reports |
| IT / Operations | Execute provisioning, deprovisioning, and access reviews |
| All personnel | Use access responsibly, report misuse, protect credentials |

## Review and approval

This policy shall be reviewed at least annually by the [Owner Role] and approved by leadership. Material changes require leadership approval.

## Related SOC 2 criteria

- **CC6.1**
- **CC6.2**
- **CC6.3**
- **CC6.6**
- **CC6.7**

Definitions are available in the AICPA Trust Services Criteria document.

## Revision history

| Version | Date | Author | Summary of changes |
|---|---|---|---|
| 1.0 | [Effective Date] | [Owner Role] | Initial version |

---

Generated and maintained by SimpleAudit — https://simpleaudit.io
