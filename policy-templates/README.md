# Policy Templates

Twelve generic SOC 2 policy templates in Markdown. Each template covers a control area that virtually every SOC 2 audit will examine.

## Why written policies matter

SOC 2 is a *controls* attestation, and a policy is the documented control objective. Auditors will sample your policies for three things:

1. **Existence.** A policy must be written down, approved, and dated. An informal practice — no matter how well-followed — is not a SOC 2 control.
2. **Coverage.** The policy must address the Trust Services Criteria the auditor is testing. Each template in this directory lists which TSC sections it supports at the bottom.
3. **Alignment with practice.** The policy must describe how the organization actually operates. If your incident response policy promises a four-hour pager response and your on-call rotation is unstaffed on weekends, that is a control deficiency. Write what you do; do what you write.

## How to use these templates

1. **Read the policy in full** before editing. Compliance documents have load-bearing language; deleting a clause because it "sounds redundant" can remove the control the auditor is testing for.
2. **Replace placeholder fields.** Search for `[Company Name]`, `[Effective Date]`, `[Owner Role]`, and similar bracketed values. Each occurrence is a deliberate placeholder.
3. **Tailor to your environment.** Cloud-only SaaS companies do not need data-center physical security clauses; companies with no field staff do not need clauses about laptop encryption at customer sites. Remove what does not apply, but be careful not to remove a clause your auditor expects to see.
4. **Get approval.** Every policy must be approved by the role identified in its *Roles & Responsibilities* section (typically a member of leadership) and dated. The auditor will look at the approval date and the next-review date.
5. **Review annually.** Most policies should be reviewed at least once a year, more often if your environment changes materially. Track reviews in the *Revision history* table at the bottom of each policy.

## The twelve policies

| Policy | Primary control area | Why it matters |
|---|---|---|
| [Information Security Policy](information-security-policy.md) | Overall security program (CC1, CC2) | The "umbrella" policy that other policies refer to |
| [Access Control Policy](access-control-policy.md) | Logical access (CC6.1, CC6.2, CC6.3) | The single largest source of audit findings |
| [Acceptable Use Policy](acceptable-use-policy.md) | End-user behavior (CC1.4, CC2.2) | Sets expectations for staff; signed at hire |
| [Risk Management Policy](risk-management-policy.md) | Risk assessment (CC3.1–CC3.4) | Drives the risk register the auditor will sample |
| [Vendor Management Policy](vendor-management-policy.md) | Third-party risk (CC9.2) | Auditors always sample subservice organizations |
| [Incident Response Policy](incident-response-policy.md) | Incident handling (CC7.3, CC7.4, CC7.5) | Tested via incident records and tabletop exercises |
| [Business Continuity Policy](business-continuity-policy.md) | Availability (A1.2, A1.3) | Required if the report scope includes Availability |
| [Data Classification Policy](data-classification-policy.md) | Data handling (C1.1, CC6.7) | Required if scope includes Confidentiality |
| [Data Retention Policy](data-retention-policy.md) | Retention & disposal (CC6.5, P4.2) | Common Privacy and Confidentiality requirement |
| [Change Management Policy](change-management-policy.md) | Production changes (CC8.1) | Tested by sampling deploy and review records |
| [Vulnerability Management Policy](vulnerability-management-policy.md) | Patching & scanning (CC7.1) | Tested by reviewing scan output and remediation timelines |
| [Security Awareness Policy](security-awareness-policy.md) | Training (CC2.2, CC1.4) | Tested by sampling training completion records |

## A note on scope

The twelve policies above cover the most common SOC 2 Type 2 scope: the Security Common Criteria, plus the additional categories (Availability, Confidentiality, Processing Integrity, Privacy) most often added by SaaS organizations. If your report scope is narrower — for example, Security only — you can still adopt these policies; you just will not be evaluated against the additional category clauses. If your report scope includes Processing Integrity, you will likely also need a transaction-processing or data-integrity policy that goes beyond the templates here, because PI is highly domain-specific.

## On generative AI and AI services

Several of these templates mention AI tools and AI-augmented services. If your organization processes customer data through third-party AI services (LLMs, embedding APIs, etc.), treat those services as subprocessors under your Vendor Management Policy and verify they have appropriate attestations of their own. The auditor will ask.

---

Built and maintained by [SimpleAudit](https://simpleaudit.io) — AI-driven SOC 2 compliance.
