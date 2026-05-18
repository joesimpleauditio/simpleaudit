# Data Classification Policy

**Owner:** [Owner Role — typically CTO, CISO, or Head of Engineering]
**Effective Date:** [Effective Date]
**Next Review:** [Effective Date + 1 year]
**Version:** 1.0

## Purpose

This policy establishes a uniform scheme for classifying information at [Company Name] according to sensitivity, and defines the handling requirements for each classification level. Consistent classification enables proportionate protection — strong controls for the data that needs them, lighter handling for data that does not — and supports compliance with confidentiality and privacy obligations.

## Scope

This policy applies to all information created, received, processed, transmitted, stored, or disposed of by [Company Name], regardless of format (electronic, paper, verbal, visual) or medium (cloud, device, removable media). It applies to all personnel and third parties handling [Company Name] information.

## Policy statements

### 1. Classification levels

[Company Name] uses four classification levels. Each level has defined handling requirements:

#### 1.1. Public

**Definition.** Information intended for general distribution outside the company. Disclosure presents no risk.

**Examples.** Marketing materials, public blog posts, published policies (this repository, for instance), public statements.

#### 1.2. Internal

**Definition.** Information used to operate the company and not intended for public distribution. Disclosure would cause minor inconvenience but no material damage.

**Examples.** Internal announcements, organizational charts, internal training materials, generic project plans.

#### 1.3. Confidential

**Definition.** Information whose unauthorized disclosure would cause material harm to [Company Name], its personnel, or its customers. Includes most business and customer data.

**Examples.** Customer data, source code (private), financials, contracts, personnel records, security configurations, third-party data shared under NDA.

#### 1.4. Restricted

**Definition.** Information whose unauthorized disclosure would cause severe harm — significant financial loss, regulatory enforcement, loss of customer trust, or competitive damage. Subject to the strictest handling requirements.

**Examples.** Customer authentication credentials, encryption keys and KMS materials, payment card data (if handled), personal data classified as sensitive under applicable law (e.g., GDPR special categories), security incident details prior to disclosure, mergers and acquisitions information, individual customer-account-level financial data.

### 2. Classification assignment

2.1. The data owner — typically the team that creates or receives the data — is responsible for assigning classification. When in doubt, classify at the higher of two plausible levels.

2.2. Customer data is, by default, classified **Confidential**. Customer data falling within Restricted examples (credentials, sensitive personal data) shall be classified **Restricted**.

2.3. Aggregated or derived data inherits the highest classification of any of its components. De-identification or aggregation may justify a lower classification only when the residual re-identification risk has been formally assessed and accepted.

2.4. The classification of a dataset shall be reviewed when its purpose, scope, or sensitivity changes.

### 3. Handling requirements

The following requirements apply at each classification level. Higher-level requirements are cumulative — Restricted handling includes all Confidential requirements, and so on.

#### 3.1. Public

- No special handling required.
- Personnel should still verify that information labeled Public has been approved for external release.

#### 3.2. Internal

- Access limited to employees, contractors, and authorized third parties under confidentiality obligations.
- May be transmitted by company email and stored on company-managed platforms.
- Should not be posted to public forums, social media, or third-party services without review.

#### 3.3. Confidential

- Access on a need-to-know basis, granted through the [Access Control Policy](access-control-policy.md).
- Transmission only through encrypted channels (TLS for email and APIs, encrypted file sharing for documents).
- Storage in approved systems with encryption at rest.
- Must not be copied to personal devices, personal cloud accounts, or removable media.
- Use with AI services only through approved providers with appropriate data-processing terms (see Acceptable Use Policy, Section 6.3).
- Hardcopy (printed materials) must be controlled and securely shredded at end of life.

#### 3.4. Restricted

- Access narrowly scoped, with documented business justification and an expiration where practical.
- Privileged access requires additional approval and MFA.
- Encryption in transit and at rest with strong, current algorithms; key management per the [Information Security Policy](information-security-policy.md).
- May not be sent in email body or unencrypted attachments; use approved secrets-sharing tools.
- May not be displayed in shared screens during meetings without explicit awareness; use redaction where practical.
- May not be processed in development or test environments unless de-identified or specifically authorized.
- Access is logged, and access patterns are monitored.

### 4. Labeling

4.1. **Documents** — Internal, Confidential, and Restricted documents should be labeled in headers or footers (or in the document title where supported).

4.2. **Email** — Subject lines may be prefixed with the classification level for Confidential and Restricted communications, especially when forwarding outside the originating team.

4.3. **Data stores** — Databases, tables, and storage buckets containing Confidential or Restricted data shall be tagged or organized so the classification is discoverable from the system catalog or naming convention.

### 5. Retention and disposal

5.1. Information shall be retained per the [Data Retention Policy](data-retention-policy.md). Classification does not, by itself, determine retention; it determines how the data is handled while retained and how it is disposed of.

5.2. Disposal of Confidential and Restricted information shall use methods that prevent recovery:
- Electronic media: cryptographic erasure, secure-delete tooling, or physical destruction
- Cloud storage: delete with appropriate audit logging
- Hardcopy: cross-cut shredding or equivalent

5.3. Disposal records (the system, the data, the actor, the date, the method) shall be retained for at least three (3) years for Confidential and Restricted data.

### 6. Sharing with third parties

6.1. Confidential and Restricted information may be shared with third parties only under appropriate contractual protections (NDA, data-processing agreement) and through approved channels.

6.2. Customer data shared with subprocessors is governed by the [Vendor Management Policy](vendor-management-policy.md).

### 7. Exceptions

7.1. Exceptions to handling requirements require written approval from the [Owner Role], documented compensating controls, and an expiration date. Exceptions for Restricted data require leadership approval.

### 8. Training and awareness

8.1. Personnel shall be trained on this policy as part of onboarding and at least annually thereafter. Training shall include practical examples of correct handling at each classification level.

## Roles and responsibilities

| Role | Responsibility |
|---|---|
| [Owner Role] | Maintain this policy; resolve classification disputes |
| Data owners | Assign and review classifications for data they own |
| All personnel | Handle data per its classification; raise classification questions |
| IT / Security | Provide tooling that enforces handling requirements |
| Legal | Identify legal/regulatory drivers of classification (e.g., sensitive personal data) |

## Review and approval

This policy shall be reviewed at least annually by the [Owner Role] and approved by leadership. Material changes require leadership approval.

## Related SOC 2 criteria

- **CC6.1, CC6.7** — Logical access controls and restriction of information transmission
- **CC6.5** — Disposal of information
- **C1.1** — Information designated as confidential is protected to meet objectives (Confidentiality category)
- **C1.2** — Confidential information is disposed of in accordance with objectives
- **P1.1, P3.1, P4.2** (Privacy category) — When sensitive personal data is within scope

## Revision history

| Version | Date | Author | Summary of changes |
|---|---|---|---|
| 1.0 | [Effective Date] | [Owner Role] | Initial version |

---

Generated and maintained by SimpleAudit — https://simpleaudit.io
