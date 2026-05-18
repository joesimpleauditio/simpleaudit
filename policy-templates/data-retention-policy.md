# Data Retention Policy

**Owner:** [Owner Role — typically Legal Counsel, COO, or CTO]
**Effective Date:** [Effective Date]
**Next Review:** [Effective Date + 1 year]
**Version:** 1.0

## Purpose

This policy establishes how long [Company Name] retains different categories of information, when and how that information is disposed of, and how retention obligations are reconciled with privacy expectations and legal holds. The objectives are: meet legal, regulatory, and contractual obligations; minimize storage and privacy risks by not keeping data longer than necessary; and preserve the records required for business, audit, and dispute purposes.

## Scope

This policy applies to all information created, received, processed, or stored by [Company Name], regardless of format or system. It applies to all personnel and to vendors handling data on [Company Name]'s behalf. It supplements and is governed by the [Data Classification Policy](data-classification-policy.md).

## Policy statements

### 1. Retention principles

1.1. [Company Name] retains information only as long as necessary for the purpose for which it was collected, plus any legally or contractually mandated period.

1.2. Where retention periods are not externally mandated, [Company Name] sets a default period appropriate to business need and reviews it periodically.

1.3. Personal data is retained only as long as necessary for the documented processing purposes. When the purpose is met, the data is deleted, anonymized, or moved to an archive with corresponding access restrictions.

1.4. Data that has reached the end of its retention period is disposed of promptly through methods appropriate to its classification.

### 2. Retention schedule

The retention schedule below is the default. Where customer contracts or regulations require a longer period, the longer period applies.

| Data category | Default retention | Notes |
|---|---|---|
| Customer-uploaded content | Active life of contract + 30 days; per-customer overrides per contract | Deletion confirmed within 30 days of contract end or earlier customer-initiated deletion |
| Customer account metadata (org, user records) | Active life of contract + 90 days | Allows for reactivation; subsequent deletion |
| Production application logs (security-relevant) | 1 year minimum, 2 years typical | Cold storage acceptable after 90 days |
| Production application logs (debug, non-security) | 30–90 days | Per system |
| Authentication and audit logs | 2 years minimum | Required for security monitoring and incident investigation |
| Backups of production data | Per backup tier; commonly 30–90 days for online, 1 year for long-term | Aligned with RPO and customer commitments |
| Incident records and post-incident reviews | 5 years | Compliance and trend analysis |
| Vendor due diligence records | Duration of relationship + 3 years | Per Vendor Management Policy |
| Access review records | 2 years | Per Access Control Policy |
| Personnel records (employees, contractors) | Per local employment law; commonly 7 years post-separation in the U.S. | Sensitive subsets handled separately |
| Email and chat | 1 year default, with extensions for specific business purposes | Subject to legal hold |
| Financial records | 7 years | Tax and audit requirements |
| Contracts and DPAs | Duration of agreement + 6 years | Statute of limitations |
| Marketing data and prospect records | 2 years from last interaction | Subject to opt-out preferences |

2.1. The retention schedule shall be reviewed at least annually and updated as business needs, regulations, and contracts change.

### 3. Customer data deletion

3.1. Customer data shall be deleted from production systems within thirty (30) days of contract termination, unless the contract specifies a different period.

3.2. Customer data shall also be deleted from backups in the next backup cycle following the production deletion, or per the backup retention schedule, whichever is shorter. Customer-initiated deletion before contract end follows the same backup-cycle timing.

3.3. Confirmation of deletion shall be provided to the customer upon written request and shall be retained as evidence of the deletion event.

3.4. Where regulatory or legitimate-business needs require retention beyond contract termination (e.g., financial records of the transaction itself), only the minimum necessary information shall be retained, and customer data subject to the retention exception shall be documented.

### 4. Personal data subject rights

4.1. Where applicable law grants individuals rights to access, rectify, or delete personal data (GDPR, CCPA, and similar regimes), [Company Name] shall honor those rights within the timelines required by the applicable law.

4.2. Verified erasure requests trigger deletion of the subject's personal data from production and from backups per Section 3.2, subject to exceptions for retention required by law or for the establishment, exercise, or defense of legal claims.

### 5. Legal holds

5.1. When [Company Name] reasonably anticipates litigation, government investigation, or regulatory inquiry, Legal Counsel shall issue a **legal hold** suspending normal retention for the relevant records.

5.2. Personnel who receive a legal hold notice shall preserve responsive information until the hold is lifted, regardless of normal retention schedules. Automated deletion processes shall be suspended for affected data sets.

5.3. Legal holds shall be tracked centrally with effective date, scope, custodians, and release date. Records of holds (issued, modified, released) shall be retained for at least seven (7) years.

### 6. Disposal

6.1. Disposal methods shall be appropriate to classification:
- **Public, Internal** — standard deletion or shredding
- **Confidential** — secure delete, cryptographic erasure, or physical destruction; verified
- **Restricted** — methods preventing recovery; verified and documented

6.2. Cloud storage disposal shall rely on the provider's documented deletion guarantees. For customer-managed encryption keys, key destruction is an acceptable substitute for data deletion (cryptographic erasure) where supported by the system.

6.3. Disposal records shall include date, system, data scope, method, actor, and verification. Records for Confidential and Restricted data shall be retained for at least three (3) years.

### 7. Backups and archives

7.1. Retention of backups follows separate schedules tied to recovery objectives. Backups may temporarily retain information past the production retention period; this is acceptable provided the data is destroyed within the next backup cycle aligned with retention end.

7.2. Archives (long-term storage of records preserved for business or legal reasons) shall be subject to access controls equivalent to the underlying data's classification.

### 8. Vendor data retention

8.1. Vendors handling customer data on [Company Name]'s behalf shall be contractually required to delete data per [Company Name]'s schedule or shorter, and to provide deletion confirmation on request.

8.2. Vendor retention practices shall be verified during annual reviews.

### 9. Exceptions

9.1. Retention exceptions (extensions beyond schedule, early deletion) require written approval from [Owner Role]. Legal Counsel shall be consulted for exceptions involving personal data or potentially litigation-relevant records.

### 10. Records of retention activity

10.1. Records of deletion activities, legal holds, and exceptions shall themselves be retained per this policy (typically five (5) years), supporting auditability of the retention program.

## Roles and responsibilities

| Role | Responsibility |
|---|---|
| [Owner Role] | Maintain this policy and the retention schedule |
| Legal Counsel | Issue and release legal holds; advise on regulatory retention |
| Data owners | Implement retention controls in their systems |
| IT / Engineering | Configure systems to enforce retention; perform automated deletions |
| Vendors | Comply with contractual retention obligations |

## Review and approval

This policy and its retention schedule shall be reviewed at least annually by the [Owner Role] in consultation with Legal Counsel, and approved by leadership.

## Related SOC 2 criteria

- **CC6.5** — Identifies, develops, and implements activities to prevent and remediate the loss of information through retention and disposal
- **C1.2** — Confidential information is disposed of in accordance with objectives
- **P4.1, P4.2, P4.3** (Privacy) — Retention and disposal of personal information

## Revision history

| Version | Date | Author | Summary of changes |
|---|---|---|---|
| 1.0 | [Effective Date] | [Owner Role] | Initial version |

---

Generated and maintained by SimpleAudit — https://simpleaudit.io
