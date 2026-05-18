# Vendor Management Policy

**Owner:** [Owner Role — typically COO, CTO, or Head of Procurement / Operations]
**Effective Date:** [Effective Date]
**Next Review:** [Effective Date + 1 year]
**Version:** 1.0

## Purpose

This policy governs how [Company Name] selects, onboards, monitors, and offboards third-party vendors that process company or customer data, deliver critical services, or otherwise materially affect the security or availability of [Company Name]'s products and operations. The objective is to ensure that risks introduced by third parties are identified and managed to a level consistent with the company's risk appetite.

## Scope

This policy applies to all third-party relationships involving:
- Storage, transmission, or processing of [Company Name] data or customer data
- Operation of systems used to deliver [Company Name]'s products or services
- Provision of services that, if disrupted, would materially affect [Company Name]'s ability to operate

The policy applies regardless of contract size or whether the vendor is paid (free services that handle company data are within scope).

## Policy statements

### 1. Vendor inventory

1.1. [Company Name] shall maintain a vendor inventory recording, at minimum:
- Vendor name and primary contact
- Description of services
- Data types accessed or processed
- Risk tier (see Section 3)
- Subprocessor status (yes/no)
- Contract effective date and renewal date
- Date of last security review
- Internal owner

1.2. New vendors that will handle company data or provide critical services shall not be engaged until they are entered in the inventory and assessed.

### 2. Vendor categories

2.1. Vendors shall be classified into one of the following categories:
- **Subprocessors** — vendors that process customer data on [Company Name]'s behalf (cloud infrastructure, email, analytics on customer data, etc.). Subprocessors are listed publicly on [Subprocessor List Location, e.g., simpleaudit.io/subprocessors] and customer-facing terms require prior notice of additions.
- **Critical service providers** — vendors whose disruption would materially affect operations, even if they do not handle customer data (payroll, source code hosting, communication tools).
- **Standard vendors** — vendors with limited access to data and non-critical services.
- **Low-risk vendors** — vendors with no access to data and minimal operational dependency.

### 3. Vendor risk tiering

3.1. Each vendor shall be assigned a risk tier based on factors including:
- Sensitivity of data accessed or processed
- Operational criticality
- Volume of data
- Geographic location of data processing
- Public-facing status (subprocessor or not)

3.2. Tiering drives the depth of pre-engagement assessment and the frequency of ongoing review.

### 4. Pre-engagement due diligence

4.1. Before contracting with a vendor in the **Subprocessor** or **Critical service provider** categories, [Company Name] shall:
- Obtain a current SOC 2 Type 2 report (or equivalent, such as ISO 27001 certification, or, for smaller vendors, a completed security questionnaire) — verify scope, applicable Trust Services Categories, and the period covered
- Review any reported control exceptions and any compensating controls
- Identify subservice organizations carved out of the report and assess that the carve-out is acceptable
- Verify business continuity and incident response capabilities through documented procedures and, where appropriate, testing records
- Confirm appropriate contractual terms: data-processing addendum, security obligations, breach notification timelines, audit rights, indemnification

4.2. Where a vendor lacks a SOC 2 or equivalent attestation, the [Owner Role] shall document a risk acceptance with compensating controls, or the vendor shall not be engaged for in-scope use.

4.3. Due diligence records shall be retained for at least the duration of the relationship plus three (3) years.

### 5. Contracting

5.1. Contracts with vendors handling customer data shall include, at minimum:
- A data-processing agreement (DPA) defining purposes of processing, data categories, retention, and deletion obligations
- Confidentiality terms
- Security obligations consistent with [Company Name]'s control environment
- Incident notification timelines (typically forty-eight (48) hours from discovery of confirmed incidents affecting [Company Name] data)
- Audit and assurance rights (e.g., right to receive annual SOC 2 reports)
- Restrictions on further subprocessing without notice
- Data return and destruction obligations upon termination

5.2. Contracts with critical service providers shall include service level commitments appropriate to the criticality of the service.

### 6. Ongoing monitoring

6.1. Each in-scope vendor shall be reviewed at least annually. The review shall confirm:
- A current SOC 2 (or equivalent) report has been received and reviewed
- No material changes have occurred in the vendor's operations, ownership, or subservice organizations
- No unresolved exceptions affect [Company Name]'s controls
- The vendor's incident and breach history is acceptable
- Pricing, usage, and need still justify the relationship

6.2. Critical findings from a vendor's audit report shall be tracked to remediation or risk-accepted with documented compensating controls.

6.3. Public reports of vendor security incidents shall be evaluated promptly to determine impact on [Company Name].

### 7. Subprocessor changes

7.1. Additions or removals of subprocessors that process customer data shall be:
- Communicated to customers per contractual notice periods (typically thirty (30) days advance notice)
- Reflected on the public subprocessor list
- Recorded in the vendor inventory before the change takes effect

### 8. Vendor incident response

8.1. Suspected or confirmed incidents involving a vendor shall be managed under the [Incident Response Policy](incident-response-policy.md), with the vendor's incident-response team engaged through documented channels.

8.2. Vendor incidents involving customer data shall be evaluated for notification obligations to customers and regulators.

### 9. Termination and offboarding

9.1. Upon termination of a vendor relationship, [Company Name] shall:
- Confirm return or destruction of [Company Name] data per contract
- Disable vendor access to [Company Name] systems within five (5) business days of termination
- Remove the vendor from the active inventory and retain records per Section 4.3
- Notify customers if the offboarded vendor was a listed subprocessor

### 10. Internal AI subprocessors

10.1. AI and machine-learning services that process customer data shall be treated as subprocessors under this policy. The data-processing agreement shall explicitly address use of customer data for model training, telemetry, and prompt retention. Services that train on customer data without an opt-out shall not be used with classified data.

## Roles and responsibilities

| Role | Responsibility |
|---|---|
| [Owner Role] | Maintain this policy; approve subprocessor additions |
| Internal vendor owners | Manage the day-to-day relationship; raise issues; perform reviews |
| Legal / Procurement | Negotiate contracts and DPAs |
| Security / Compliance | Conduct security assessments; review SOC 2 reports |
| Finance | Track contract renewals |

## Review and approval

This policy shall be reviewed at least annually by the [Owner Role] and approved by leadership. Material changes require leadership approval.

## Related SOC 2 criteria

- **CC2.3** — Communication with external parties
- **CC9.2** — Assesses and manages risks associated with vendors and business partners
- **CC6.1** (indirect) — Access by third parties is governed by the access control program

## Revision history

| Version | Date | Author | Summary of changes |
|---|---|---|---|
| 1.0 | [Effective Date] | [Owner Role] | Initial version |

---

Generated and maintained by SimpleAudit — https://simpleaudit.io
